# 19 — Entity Framework Core

**EF Core** is .NET's ORM (Object-Relational Mapper): it maps your C# classes to database tables so you work with objects and LINQ instead of raw SQL. It's the default data-access tool for the vast majority of C# CRUD apps. This chapter gets you productive with the parts you'll use daily.

## The model: entities and `DbContext`

Your **entities** are plain C# classes representing tables. The **`DbContext`** is your gateway to the database — it exposes `DbSet<T>` properties (one per table) and tracks changes.

```csharp
public class Customer
{
    public int Id { get; set; }                      // convention: 'Id' or 'CustomerId' = primary key
    public string Name { get; set; } = "";
    public string Email { get; set; } = "";
    public DateTime CreatedAt { get; set; }

    // Navigation property — the relationship to orders:
    public List<Order> Orders { get; set; } = new();
}

public class Order
{
    public int Id { get; set; }
    public decimal Total { get; set; }
    public DateTime PlacedAt { get; set; }

    public int CustomerId { get; set; }              // foreign key (convention)
    public Customer Customer { get; set; } = null!;  // navigation back to the customer
}
```

```csharp
using Microsoft.EntityFrameworkCore;

public class ShopDbContext : DbContext
{
    public ShopDbContext(DbContextOptions<ShopDbContext> options) : base(options) { }

    public DbSet<Customer> Customers => Set<Customer>();
    public DbSet<Order> Orders => Set<Order>();

    // Configure mappings/constraints not covered by convention:
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Customer>(e =>
        {
            e.Property(c => c.Name).IsRequired().HasMaxLength(200);
            e.Property(c => c.Email).IsRequired().HasMaxLength(320);
            e.HasIndex(c => c.Email).IsUnique();
        });

        modelBuilder.Entity<Order>()
            .Property(o => o.Total)
            .HasPrecision(18, 2);     // decimal precision for money
    }
}
```

EF Core uses **conventions** heavily: `Id`/`{Type}Id` → primary key; `CustomerId` + `Customer` nav → foreign key relationship. You only configure (via fluent API in `OnModelCreating` or `[attributes]`) what conventions don't cover.

## Registering EF Core with DI

In `Program.cs`, register the context (scoped by default — one per request):

```csharp
builder.Services.AddDbContext<ShopDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("Default")));
    // or .UseNpgsql(...) for PostgreSQL, .UseSqlite(...) for SQLite
```

Then inject `ShopDbContext` wherever you need data access. **The `DbContext` is scoped and NOT thread-safe** — never share one instance across threads or `await` two queries on it concurrently.

## Migrations — evolving the schema

You define the model in C#; **migrations** generate and apply the matching SQL schema. This is "code-first." Install the tools once (`dotnet tool install --global dotnet-ef`) and the `Microsoft.EntityFrameworkCore.Design` package.

```bash
# Create a migration from the current model (generates a C# migration file):
dotnet ef migrations add InitialCreate

# Apply pending migrations to the database (creates/updates tables):
dotnet ef database update

# After changing the model, add another migration and update again:
dotnet ef migrations add AddOrderStatus
dotnet ef database update

# Undo the last (unapplied) migration:
dotnet ef migrations remove
```

Migrations are checked into source control and run as part of deployment. They give you a versioned, reviewable history of schema changes. Never hand-edit the database out from under EF.

## CRUD operations

The four core operations, all async (always use the async methods in web apps):

### Create

```csharp
public async Task<Customer> AddCustomerAsync(string name, string email)
{
    var customer = new Customer { Name = name, Email = email, CreatedAt = DateTime.UtcNow };
    _db.Customers.Add(customer);
    await _db.SaveChangesAsync();    // INSERT happens here; customer.Id is populated
    return customer;
}
```

Nothing hits the database until **`SaveChangesAsync()`**. EF tracks the new entity and issues the INSERT then; it also fills in the generated `Id`.

### Read (query with LINQ)

```csharp
// By primary key (fastest):
Customer? c = await _db.Customers.FindAsync(id);

// Single with a predicate:
Customer? byEmail = await _db.Customers
    .FirstOrDefaultAsync(c => c.Email == email);

// Filtered, sorted, projected list:
List<CustomerDto> recent = await _db.Customers
    .Where(c => c.CreatedAt >= cutoff)
    .OrderByDescending(c => c.CreatedAt)
    .Select(c => new CustomerDto(c.Id, c.Name, c.Email))   // project to DTO
    .ToListAsync();

// Pagination:
var page = await _db.Customers
    .OrderBy(c => c.Name)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .ToListAsync();

bool exists = await _db.Customers.AnyAsync(c => c.Email == email);
int count = await _db.Orders.CountAsync(o => o.Total > 100);
```

**This LINQ runs as SQL on the database**, not in memory — EF translates your query to SQL, sends it, and materializes the results. That's the magic: same LINQ you learned in Chapter 10, executed by the DB. (More on this critical point below.)

### Update

```csharp
public async Task UpdateEmailAsync(int id, string newEmail)
{
    var customer = await _db.Customers.FindAsync(id);
    if (customer is null) throw new NotFoundException($"Customer {id}");

    customer.Email = newEmail;       // just mutate the tracked entity
    await _db.SaveChangesAsync();    // EF detects the change and issues UPDATE
}
```

EF's **change tracking** notices which properties of loaded entities changed and generates the minimal UPDATE. You don't write UPDATE statements — you load, mutate, save.

### Delete

```csharp
public async Task DeleteAsync(int id)
{
    var customer = await _db.Customers.FindAsync(id);
    if (customer is null) return;
    _db.Customers.Remove(customer);
    await _db.SaveChangesAsync();    // DELETE
}

// Bulk delete without loading (EF Core 7+) — efficient for many rows:
await _db.Orders.Where(o => o.PlacedAt < oldDate).ExecuteDeleteAsync();
await _db.Orders.Where(o => o.Total == 0).ExecuteUpdateAsync(
    s => s.SetProperty(o => o.Total, 0.01m));
```

## Loading related data

Navigation properties (`customer.Orders`) aren't loaded automatically. You **opt in** with `Include`:

```csharp
// Eager loading — load customer WITH their orders in one query (a JOIN):
var customer = await _db.Customers
    .Include(c => c.Orders)
    .FirstOrDefaultAsync(c => c.Id == id);

// Nested includes:
var customer2 = await _db.Customers
    .Include(c => c.Orders)
        .ThenInclude(o => o.Lines)
    .FirstOrDefaultAsync(c => c.Id == id);
```

### The N+1 problem (a classic performance bug)

If you load a list and then access each item's navigation property in a loop *without* `Include`, EF fires a separate query per item — 1 query for the list + N for the children = "N+1 queries." On a list of 100, that's 101 round-trips.

```csharp
// ❌ N+1: one query for customers, then one query per customer for its orders
var customers = await _db.Customers.ToListAsync();
foreach (var c in customers)
{
    int orderCount = await _db.Orders.CountAsync(o => o.CustomerId == c.Id);  // 1 query per loop
    Console.WriteLine(orderCount);
}

// ⚠️ The tempting version — c.Orders.Count without Include — is not N+1 by default:
// lazy loading is OFF unless you install Microsoft.EntityFrameworkCore.Proxies and
// call UseLazyLoadingProxies(). Without it, c.Orders is just empty and prints 0 —
// a silent wrong answer. With lazy loading on, it becomes the classic N+1.

// ✅ One query with the join:
var customers = await _db.Customers.Include(c => c.Orders).ToListAsync();
```

Watch for this — it's one of the most common EF performance problems in real apps. Better still, **project to a DTO** with exactly the fields you need so EF builds an efficient query:

```csharp
var data = await _db.Customers
    .Select(c => new { c.Name, OrderCount = c.Orders.Count })  // efficient single query
    .ToListAsync();
```

## `AsNoTracking` for read-only queries

By default EF tracks every entity it returns (to detect changes). For read-only queries (you won't modify and save), disable tracking — it's faster and uses less memory:

```csharp
var products = await _db.Products
    .AsNoTracking()
    .Where(p => p.IsActive)
    .ToListAsync();
```

Use `AsNoTracking()` for GET endpoints and reports. Use tracked queries when you intend to modify and `SaveChanges`.

## Client vs server evaluation — the crucial gotcha

EF translates LINQ to SQL. If you call something it **can't** translate, older EF (before Core 3.0) would silently run it in memory (slow); modern EF **throws** instead. The one exception is the *final* `Select`: EF Core will run an untranslatable method there on the client after fetching the rows — but a `Where`/`OrderBy` it can't translate always throws. Rule: **keep `Where`/`OrderBy`/`Select` expressions translatable** (simple comparisons, string methods EF knows). If you need a C# method it can't translate, materialize first with `ToListAsync()`, then do the in-memory work with LINQ-to-Objects:

```csharp
// This C# method can't be turned into SQL:
var result = await _db.Customers
    .Where(c => c.IsActive)              // translatable -> runs in SQL
    .ToListAsync();                      // materialize
var formatted = result                   // now in memory
    .Select(c => MyComplexFormat(c))     // C# method, fine in memory
    .ToList();
```

Know *where* your query stops being SQL and starts being C#.

## Transactions and `SaveChanges`

A single `SaveChangesAsync()` is atomic — all tracked changes succeed or roll back together. For multiple `SaveChanges` calls that must be atomic, use an explicit transaction:

```csharp
await using var tx = await _db.Database.BeginTransactionAsync();
try
{
    _db.Orders.Add(order);
    await _db.SaveChangesAsync();
    _db.Inventory.Update(item);
    await _db.SaveChangesAsync();
    await tx.CommitAsync();
}
catch
{
    await tx.RollbackAsync();
    throw;
}
```

## Concurrency (brief but important)

If two users edit the same row, the second can overwrite the first ("lost update"). EF supports **optimistic concurrency** via a `[Timestamp]`/`rowversion` column: on save, EF checks the row hasn't changed since you loaded it and throws `DbUpdateConcurrencyException` if it has, so you can handle the conflict. On SQL Server, add a `public byte[] RowVersion { get; set; } = null!;` with `.IsRowVersion()` configuration when you need it; the database bumps it on every update. SQLite has no `rowversion` type, so there you mark an ordinary property (e.g. an `int Version`) with `.IsConcurrencyToken()` and increment it yourself before saving. Just know the concept exists — naive "load, edit, save" has a race condition under concurrency.

## Practical patterns

- **Inject `DbContext` directly into services**, or wrap it in a repository if your team prefers. The context already *is* a unit-of-work + repository; don't over-abstract. Both approaches are common.
- **Return DTOs/records from your API, not entities** — projecting with `Select` avoids over-fetching, prevents serialization cycles (Customer→Orders→Customer), and decouples your API shape from your DB schema.
- **For dev/learning, SQLite is frictionless** (`UseSqlite("Data Source=shop.db")`) — no server to install. Switch the provider for production (SQL Server, PostgreSQL).
- **For tests**, use SQLite in-memory or the EF in-memory provider, or (best) a real DB via Testcontainers (Chapter 21).

## A complete service example

```csharp
public class CustomerService(ShopDbContext db, ILogger<CustomerService> logger)
{
    public async Task<CustomerDto?> GetAsync(int id, CancellationToken ct = default)
        => await db.Customers
            .AsNoTracking()
            .Where(c => c.Id == id)
            .Select(c => new CustomerDto(c.Id, c.Name, c.Email))
            .FirstOrDefaultAsync(ct);

    public async Task<CustomerDto> CreateAsync(CreateCustomer request, CancellationToken ct = default)
    {
        if (await db.Customers.AnyAsync(c => c.Email == request.Email, ct))
            throw new ConflictException($"Email {request.Email} already exists");

        var customer = new Customer
        {
            Name = request.Name,
            Email = request.Email,
            CreatedAt = DateTime.UtcNow
        };
        db.Customers.Add(customer);
        await db.SaveChangesAsync(ct);
        logger.LogInformation("Created customer {CustomerId}", customer.Id);
        return new CustomerDto(customer.Id, customer.Name, customer.Email);
    }
}
```

## Exercises

1. Set up a SQLite-backed `ShopDbContext` with `Customer` and `Order`, add a migration, and apply it.
2. Implement full CRUD for `Customer` (add, get by id, list with pagination, update email, delete) — all async, projecting to a DTO on reads.
3. Create a customer with three orders, then load the customer *with* orders using `Include`, and print the total. Then write the same as an efficient projection.
4. Demonstrate the N+1 problem: turn on query logging (`options.LogTo(Console.WriteLine)`), seed 5 customers, and run the per-customer `CountAsync` loop above — you should see 6 SELECTs logged. Rewrite it as the `Select(c => new { c.Name, OrderCount = c.Orders.Count })` projection and confirm the log shows a single SELECT. Then try `c.Orders.Count` without `Include` and explain why it prints 0.

---

**Next:** [20 — ASP.NET Core Web API](./20-aspnet-core-web-api.md)
