# 15 — Nullable Reference Types

`NullReferenceException` ("NRE") is historically the most common runtime crash in C#. **Nullable Reference Types** (NRT) is a compiler feature that catches most null bugs *at compile time*. It's enabled by default in new projects (`<Nullable>enable</Nullable>` in the `.csproj`). Understanding it cleanly separates juniors from mid-levels.

## The core idea

With NRT enabled, the compiler treats reference types as **non-null by default**, and you opt into nullability with `?`:

```csharp
string name = "Sam";       // non-nullable: the compiler assumes it's never null
string? maybe = null;      // nullable: explicitly allowed to be null

name = null;               // ⚠️ compiler WARNING: assigning null to non-nullable
maybe = null;              // fine
```

This mirrors how value-type nullability already works (`int` vs `int?`), now extended to reference types. It's a **compile-time analysis** — at runtime, `string` and `string?` are the same type. The `?` is a signal to the compiler (and to you and your teammates) about *intent*.

## Why it matters

The compiler now warns you whenever you might dereference null:

```csharp
string? GetName() => null;

string name = GetName();        // ⚠️ warning: GetName might return null
Console.WriteLine(name.Length); // ⚠️ warning: 'name' may be null here -> potential NRE
```

These warnings are your chance to fix null bugs *before* they reach production. Treat them seriously. Many teams set `<TreatWarningsAsErrors>` or specifically promote nullable warnings to errors so they can't be ignored.

## Working with the compiler's flow analysis

The compiler tracks nullability through your code. Once you've checked for null, it knows the value is safe:

```csharp
string? input = Console.ReadLine();

// After this guard, the compiler knows 'input' is non-null below:
if (input is null)
{
    Console.WriteLine("No input");
    return;
}
Console.WriteLine(input.Length);   // no warning — flow analysis proved it's non-null

// Or with null-conditional + coalescing:
int length = input?.Length ?? 0;   // safe, no warning
```

This is "flow analysis" — the compiler reasons about which paths can be null. Write natural guard clauses and the warnings disappear.

## The operators that go with it

```csharp
string? name = GetName();

// ?? — provide a non-null fallback (result is non-null):
string safe = name ?? "Anonymous";

// ?. — short-circuit member access (result is nullable):
int? len = name?.Length;

// ??= — assign only if null:
name ??= "default";

// Combine:
string city = customer?.Address?.City ?? "Unknown";
```

## The null-forgiving operator `!`

`x!` tells the compiler "I know better — treat this as non-null here," suppressing the warning. Use it **sparingly** and only when you're genuinely certain:

```csharp
string? maybe = GetValidatedName();
string definitely = maybe!;          // "trust me, it's not null"

// Common legit uses:
_field = null!;                       // will be set before use (e.g., by a framework/DI)
var item = list.FirstOrDefault(x => x.Id == id)!;  // you know it exists
// (First(...) needs no '!' — it already returns non-null T and throws if nothing matches)
```

**`!` does not change runtime behavior** — it only silences the compiler. If you're wrong, you still get an NRE at runtime. Overusing `!` defeats the entire feature. Each `!` should make you ask "can I restructure so this is provably non-null instead?"

## Designing APIs with nullability

NRT lets your method signatures *document* null contracts, and the compiler enforces them:

```csharp
// "Always returns a customer or throws" — caller doesn't need to null-check:
public Customer GetById(int id) =>
    _store.Find(id) ?? throw new NotFoundException($"Customer {id}");

// "Might not find one" — the ? forces the caller to handle null:
public Customer? FindById(int id) => _store.Find(id);

// Caller is now guided by the compiler:
Customer c = GetById(5);             // safe to use directly
Customer? maybe = FindById(5);
Console.WriteLine(maybe?.Name);      // compiler insists you handle null
```

This is a real design tool: returning `Customer?` vs `Customer` communicates intent and makes the compiler enforce correct handling. Use `Find...`/`...OrDefault` naming for the nullable variants.

## Initializing non-nullable members

A non-nullable property must be assigned, or the compiler warns. Options:

```csharp
public class Customer
{
    public string Name { get; set; } = "";          // default value
    public required string Email { get; init; }       // force caller to set it
    public List<Order> Orders { get; } = new();        // initialize collections
    public Address? BillingAddress { get; set; }       // genuinely optional -> nullable
}
```

- Give non-null members a default (`= ""`, `= new()`), **or**
- Mark them `required` so the compiler forces construction-time assignment, **or**
- If something is genuinely optional, make it nullable (`?`) and handle it.

**Initialize collection properties** to empty (`= new()`) rather than leaving them null — "no items" should be an empty list, not null. This avoids a whole category of NREs.

## Nullable value types vs nullable reference types

They look the same (`?`) but differ under the hood:
- `int?` is a real different type (`Nullable<int>`), a struct wrapping a value + a `HasValue` flag. It exists at runtime.
- `string?` is the *same* type as `string` at runtime; the `?` is purely compile-time metadata.

For value types you use `.HasValue`, `.Value`, and `??`:

```csharp
int? age = GetAge();
if (age.HasValue) Console.WriteLine(age.Value);
int safeAge = age ?? 0;
int? doubled = age * 2;          // null if age is null (lifted operator)
```

## Practical adoption notes

- New projects have it on by default — keep it on.
- When you see a nullable warning, **fix the root cause** (add a guard, change the return type, initialize the field) rather than slapping `!` everywhere.
- Pay attention to nullability when consuming external data (JSON, DB, user input) — those are exactly where nulls sneak in.
- `#nullable disable` / `#nullable enable` can toggle it per-file when migrating legacy code, but you'll rarely need that on greenfield work.

## The payoff

Used well, NRT means `NullReferenceException` becomes rare in your code — the compiler catches the mistakes for you. It's one of the highest-value features for writing robust CRUD/backend code, where data flows in from databases and HTTP and nulls are everywhere.

## Exercises

1. Take a method `string FindCity(int customerId)` that may return null and make its nullability honest; then fix all the call sites the compiler complains about.
2. Write a `Customer` class with `required` email, a defaulted name, an initialized `Orders` list, and a genuinely-nullable `MiddleName`. Make it compile warning-free.
3. Find a place where you'd be tempted to use `!` and rewrite it with a guard clause so `!` isn't needed.
4. Write `string Display(Address? address)` that returns `"No address"` when null and a formatted string otherwise, with zero nullable warnings.

---

**Next:** [16 — File I/O & JSON](./16-file-io-and-json.md)
