# 10 - Data Access with EF Core

Entity Framework Core is the main object-relational mapper for .NET. It maps C# entities to database tables and LINQ queries to SQL.

```csharp
public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)
{
    public DbSet<TaskItem> Tasks => Set<TaskItem>();
}
```

Register it with DI:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlite(builder.Configuration.GetConnectionString("Default")));
```

Use migrations to evolve schema:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

EF Core is not a replacement for database understanding. You still need SQL, indexes, constraints, transactions, and performance awareness.

## Exercises

1. Add EF Core SQLite package.
2. Create a DbContext.
3. Add an entity.
4. Create and apply a migration.
5. Query with `AsNoTracking`.
