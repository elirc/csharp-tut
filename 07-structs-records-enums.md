# 07 — Structs, Records & Enums

Three more type-defining tools. **Enums** you'll use daily. **Records** are the modern way to model data and you'll use them a lot. **Structs** you'll use occasionally and need to *understand* even when you don't write them.

## Enums — named constants

An `enum` defines a type with a fixed set of named values. Use them instead of "magic" strings or ints for fixed categories.

```csharp
public enum OrderStatus
{
    Pending,        // 0
    Paid,           // 1
    Shipped,        // 2
    Delivered,      // 3
    Cancelled       // 4
}

OrderStatus status = OrderStatus.Pending;

if (status == OrderStatus.Paid)
    Console.WriteLine("Ready to ship");

// In a switch expression:
string label = status switch
{
    OrderStatus.Pending => "Awaiting payment",
    OrderStatus.Shipped => "On its way",
    _ => status.ToString()
};
```

Why enums beat magic values:
- Self-documenting (`OrderStatus.Shipped` vs `2`).
- The compiler restricts values to the defined set.
- Refactor-friendly and discoverable via IntelliSense.

### Underlying values and conversions

Enums are backed by integers (default `int`, starting at 0). You can set explicit values:

```csharp
public enum HttpStatus
{
    Ok = 200,
    NotFound = 404,
    ServerError = 500
}

int code = (int)HttpStatus.NotFound;        // 404
HttpStatus s = (HttpStatus)200;             // HttpStatus.Ok
string name = HttpStatus.Ok.ToString();     // "Ok"

// Parsing from string (e.g., from config or an API):
if (Enum.TryParse<OrderStatus>("Paid", out var parsed))
    Console.WriteLine(parsed);              // Paid

Enum.GetValues<OrderStatus>();              // all values, for iterating
```

> Caution: a cast like `(OrderStatus)99` does **not** throw even though 99 isn't a defined member. Validate with `Enum.IsDefined` if the value comes from outside.

### `[Flags]` enums — combinable options

For bitwise combinations (a value can be several things at once), use `[Flags]` with power-of-two values:

```csharp
[Flags]
public enum Permissions
{
    None    = 0,
    Read    = 1,
    Write   = 2,
    Delete  = 4,
    All     = Read | Write | Delete    // 7
}

var p = Permissions.Read | Permissions.Write;     // combine
bool canWrite = p.HasFlag(Permissions.Write);     // true
bool canDelete = p.HasFlag(Permissions.Delete);   // false
```

You'll see `[Flags]` for things like permissions and option sets.

## Records — concise immutable data types

A **record** is a class (or struct) designed for holding data, with the compiler generating value-based equality, a readable `ToString`, deconstruction, and copy support. This is the modern default for DTOs, domain values, API models, and anything that's "just data."

```csharp
public record Person(string FirstName, string LastName, int Age);
```

That one line gives you:

```csharp
var p1 = new Person("Sam", "Lee", 30);

Console.WriteLine(p1);                    // Person { FirstName = Sam, LastName = Lee, Age = 30 }
Console.WriteLine(p1.FirstName);          // Sam  (positional params become init properties)

var p2 = new Person("Sam", "Lee", 30);
Console.WriteLine(p1 == p2);              // True! Value equality — compares all properties

// Non-destructive copy with 'with' (creates a modified copy, original unchanged):
var older = p1 with { Age = 31 };
Console.WriteLine(older);                 // Person { FirstName = Sam, LastName = Lee, Age = 31 }
Console.WriteLine(p1.Age);                // still 30

// Deconstruction:
var (first, last, age) = p1;
```

Compare to a class, where `==` would be reference equality, `ToString` would print the type name, and you'd hand-write `with`-style copies. Records eliminate all that boilerplate.

### Records are immutable by default (positional form)

The positional parameters become `init`-only properties — you can't change them after construction; you make modified copies with `with`. That immutability makes records safe to share and easy to reason about.

### Record with a body

You can add members, methods, and a full property syntax:

```csharp
public record Customer(string Name, string Email)
{
    public DateTime CreatedAt { get; init; } = DateTime.UtcNow;

    public bool HasValidEmail => Email.Contains('@');

    // You can add methods like any type:
    public string Describe() => $"{Name} <{Email}>";
}

var c = new Customer("Ann", "ann@x.com") { CreatedAt = DateTime.UtcNow };
```

### `record struct` — value-type record

`record struct` is a record that's a value type (copied by value). Use for small value-like data where you want value semantics and low allocation:

```csharp
public record struct Point(int X, int Y);          // X and Y are get; set; — mutable!
public readonly record struct Pixel(int X, int Y); // X and Y are get; init; — immutable
```

Watch the asymmetry: a positional `record` (class) gets `init`-only properties, but a positional `record struct` gets ordinary **settable** properties. Add `readonly` when you want the immutable value the guidance below recommends.

### Records vs classes — when to use which

- **Record**: immutable data, value equality desired — DTOs, API request/response models, domain value objects (Money, DateRange), events/messages.
- **Class**: entities with identity and mutable behavior, services, anything with significant logic, EF Core entities (often classes by convention).

A rough rule: if two instances with the same field values should be considered "equal," use a record. If an object has identity and behavior, use a class.

## Structs — value types you define

A `struct` is a **value type** (like `int`): copied on assignment, stored inline, no heap allocation by default, can't be `null` (unless `Nullable<T>`), and can't inherit from another struct/class (but can implement interfaces).

```csharp
public struct Coordinate
{
    public double Lat { get; init; }
    public double Lng { get; init; }

    public double DistanceTo(Coordinate other) =>
        Math.Sqrt(Math.Pow(Lat - other.Lat, 2) + Math.Pow(Lng - other.Lng, 2));
}

var a = new Coordinate { Lat = 1, Lng = 2 };
var b = a;            // COPIES the value (not a shared reference)
```

### When to use a struct

Honestly, in typical CRUD/business code, **rarely**. Use a struct only when:
- It's small (a few fields), logically a single value (a point, a money amount, a range).
- It's immutable (mutable structs are a well-known footgun — copies make mutations "disappear").
- You have performance reasons to avoid heap allocations.

Examples in the BCL: `int`, `double`, `DateTime`, `TimeSpan`, `Guid`, `decimal` are all structs. For your own data, prefer a `record` (class) unless you specifically need value-type behavior, in which case `readonly record struct` gives you the nice record features with value semantics.

### The mutable struct trap

```csharp
public struct MutablePoint { public int X; public int Y; }

var list = new List<MutablePoint> { new() { X = 1 } };
// list[0].X = 5;     // compile error — list[0] returns a COPY; mutating it is pointless
```

This kind of surprise is why the guidance is: **make structs immutable** (use `init`/readonly), or just use a record/class.

## Tuples — lightweight, ad-hoc grouping

Tuples group a few values without defining a type. Great for returning multiple values from a method or temporary grouping; **not** a replacement for real types in your domain model.

```csharp
(string name, int age) person = ("Sam", 30);
Console.WriteLine(person.name);          // Sam
Console.WriteLine(person.age);           // 30

// As a return type:
(int min, int max) GetRange(int[] xs) => (xs.Min(), xs.Max());
var (lo, hi) = GetRange(new[] { 3, 1, 9 });   // deconstruct

// Quick swap:
(a, b) = (b, a);
```

Use tuples for transient, internal results. If a grouping is meaningful and used widely, promote it to a `record`.

## Deconstruction

Records, tuples, and any type with a `Deconstruct` method can be unpacked:

```csharp
public record Person(string Name, int Age);
var (name, age) = new Person("Sam", 30);

// Add Deconstruct to a regular class to support it:
public class Money
{
    public decimal Amount { get; init; }
    public string Currency { get; init; } = "USD";
    public void Deconstruct(out decimal amount, out string currency)
        => (amount, currency) = (Amount, Currency);
}
var (amt, cur) = new Money { Amount = 9.99m };
```

## Quick decision guide

| Need | Use |
|---|---|
| Fixed set of named options | `enum` |
| Combinable options (bit flags) | `[Flags] enum` |
| Immutable data with value equality | `record` |
| Entity with identity + behavior | `class` |
| Small immutable value, perf-sensitive | `readonly record struct` / `readonly struct` |
| Throwaway multi-value group | tuple |

## Exercises

1. Model an `OrderStatus` enum and write a `switch` expression returning a user-facing message for each status.
2. Create a `Money` **record** (`decimal Amount`, `string Currency`). Show that two equal-valued instances are `==`, and use `with` to produce a copy with a different amount.
3. Make a `[Flags]` enum `FileAccess` (`Read`, `Write`, `Execute`) and test combinations with `HasFlag`.
4. Write a method returning a tuple `(bool success, string? error)` to validate a username, and deconstruct the result at the call site.

---

**Next:** [08 — Collections](./08-collections.md)
