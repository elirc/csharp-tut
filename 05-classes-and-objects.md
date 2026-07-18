# 05 — Classes & Objects

Classes are the core building block of C# programs. This chapter covers fields, properties, constructors, encapsulation, access modifiers, and `static`. Master this — almost all your real code lives in classes.

## Defining a class and creating objects

A **class** is a blueprint; an **object** (instance) is a thing built from it.

```csharp
public class Customer
{
    public string Name = "";       // a field
    public int Age;
}

// Create instances with 'new':
Customer c1 = new Customer();
c1.Name = "Sam";
c1.Age = 30;

var c2 = new Customer { Name = "Ann", Age = 25 };   // object initializer
Console.WriteLine(c2.Name);
```

The **object initializer** `{ Name = ..., Age = ... }` sets public members right after construction — very common and readable.

`new Customer()` can also be written with the **target-typed new** when the type is known from context:

```csharp
Customer c3 = new();                 // type inferred from the left side
Customer c4 = new() { Name = "Lee" };
```

## Fields

A **field** is a variable that belongs to an object (instance field) or the type (static field).

```csharp
public class BankAccount
{
    private decimal _balance;        // private field — internal state
    public readonly string AccountId; // readonly: set once (in ctor), never changes

    public BankAccount(string id)
    {
        AccountId = id;
    }
}
```

Conventions you'll see everywhere:
- Private fields are usually named `_camelCase` (leading underscore).
- `readonly` fields can only be assigned in their declaration or a constructor — good for values that shouldn't change after creation.

**Don't expose public fields** for real data — expose **properties** instead (next). Public fields give you no control over reads/writes and can't be part of an interface.

## Properties — the C# way to expose data

A **property** looks like a field from outside but is backed by `get`/`set` accessors, letting you add logic, validation, or change the implementation later without breaking callers.

```csharp
public class Person
{
    // Auto-property: the compiler generates a hidden backing field.
    public string Name { get; set; } = "";

    // Read-only from outside, settable only inside the class:
    public DateTime CreatedAt { get; private set; } = DateTime.UtcNow;

    // Computed (get-only) property — derived, no storage:
    public string Greeting => $"Hello, {Name}!";

    // Full property with validation and a backing field:
    private int _age;
    public int Age
    {
        get => _age;
        set
        {
            if (value < 0)
                throw new ArgumentOutOfRangeException(nameof(value), "Age can't be negative");
            _age = value;
        }
    }
}
```

Key points:
- `{ get; set; }` is an **auto-property** — use this by default.
- `value` is the implicit parameter in a `set`.
- A get-only computed property (`=> expression`) has no storage; it's recalculated each read.
- `{ get; private set; }` is public to read, private to write — common for invariants.

### `init` — set only during construction

`init` accessors allow setting a property in an object initializer but make it immutable afterward:

```csharp
public class Config
{
    public string Host { get; init; } = "";
    public int Port { get; init; }
}

var cfg = new Config { Host = "db", Port = 5432 };
// cfg.Port = 1;   // compile error — init-only after construction
```

`init` is the foundation of immutable data objects and pairs naturally with records (Chapter 7).

### `required` members

Mark a property `required` to force callers to set it:

```csharp
public class User
{
    public required string Email { get; init; }
    public string? DisplayName { get; init; }
}

var u = new User { Email = "a@b.com" };   // OK
// var bad = new User();                  // compile error: Email is required
```

## Constructors

A **constructor** runs when an object is created, to initialize it. It has the class's name and no return type.

```csharp
public class Rectangle
{
    public double Width { get; }
    public double Height { get; }

    public Rectangle(double width, double height)
    {
        Width = width;
        Height = height;
    }

    // Overloaded constructor that delegates to the other with `: this(...)`
    public Rectangle(double size) : this(size, size) { }

    public double Area => Width * Height;
}

var r = new Rectangle(3, 4);
var square = new Rectangle(5);     // uses the single-arg ctor -> 5x5
```

- If you write **no** constructor, C# provides a default parameterless one.
- If you write **any** constructor, the default disappears (add one back explicitly if you need it).
- `: this(...)` chains to another constructor to avoid duplicate init code.
- `: base(...)` calls a base class constructor (Chapter 6).

### Primary constructors (C# 12+)

A compact syntax declaring constructor parameters on the class itself; they're in scope throughout the class body:

```csharp
public class Rectangle(double width, double height)
{
    public double Area => width * height;     // parameters usable directly
    public double Width { get; } = width;     // capture into a property
}

var r = new Rectangle(3, 4);
```

Handy for short classes and very common with dependency injection (Chapter 18), where you inject services through the primary constructor.

## Methods on classes (instance methods)

Methods that operate on an object's state:

```csharp
public class BankAccount
{
    private decimal _balance;

    public void Deposit(decimal amount)
    {
        if (amount <= 0) throw new ArgumentException("Must be positive");
        _balance += amount;
    }

    public bool TryWithdraw(decimal amount)
    {
        if (amount <= 0 || amount > _balance) return false;
        _balance -= amount;
        return true;
    }

    public decimal GetBalance() => _balance;
}

var acct = new BankAccount();
acct.Deposit(100);
acct.TryWithdraw(30);
Console.WriteLine(acct.GetBalance());   // 70
```

`this` refers to the current instance (e.g., `this._balance`); usually optional, sometimes used to disambiguate a parameter from a field.

## Encapsulation & access modifiers

**Encapsulation** = hide internal state, expose a controlled surface. This is the whole point of classes and properties. The access modifiers:

| Modifier | Accessible from |
|---|---|
| `public` | Anywhere |
| `private` | Only within the same class (default for members) |
| `protected` | The class and its subclasses |
| `internal` | Anywhere in the same assembly (project) |
| `protected internal` | Same assembly OR subclasses |
| `private protected` | Subclasses within the same assembly |

Rules of thumb:
- Make members **as private as possible**. Expose only what callers need.
- Fields → `private`. Expose via `public` properties when needed.
- `internal` is great for "public within my project but not part of the published API."
- Top-level classes default to `internal`; members default to `private`.

```csharp
public class Order
{
    private readonly List<OrderLine> _lines = new();    // hidden internal state

    public IReadOnlyList<OrderLine> Lines => _lines;     // expose read-only view
    public decimal Total => _lines.Sum(l => l.Price);    // computed, can't be set badly

    public void AddLine(OrderLine line)                  // controlled mutation
    {
        if (line is null) throw new ArgumentNullException(nameof(line));
        _lines.Add(line);
    }
}
```

Notice the pattern: a private `List`, exposed as `IReadOnlyList` so outsiders can read but not add/remove directly, with mutation funneled through `AddLine` where you can enforce rules. This is the bread-and-butter of well-designed classes.

## Static members

`static` members belong to the **type**, not to any instance. Shared by all, accessed via the type name.

```csharp
public class Counter
{
    public static int InstanceCount;     // shared across all instances
    public int Id;

    public Counter()
    {
        InstanceCount++;                 // every new Counter bumps the shared count
        Id = InstanceCount;
    }

    // Static method — no instance needed:
    public static Counter CreateDefault() => new Counter();
}

var a = new Counter();
var b = new Counter();
Console.WriteLine(Counter.InstanceCount);   // 2  (accessed via the type)
```

- **Static methods** can't access instance members (there's no `this`).
- **Static fields** are global shared state — use sparingly; they're a common source of bugs and make testing harder.

### Static classes

A class that can't be instantiated and only holds static members. Great for stateless utilities:

```csharp
public static class MathHelpers
{
    public static int Clamp(int value, int min, int max)
        => value < min ? min : value > max ? max : value;
}

int x = MathHelpers.Clamp(150, 0, 100);   // 100
```

`Math`, `Console`, and `string.Join`-style utilities are static. The BCL has many.

### `const` and `static readonly`

```csharp
public class Circle
{
    public const double Pi = 3.14159;             // compile-time constant
    public static readonly DateTime Epoch         // runtime-initialized, never changes
        = new DateTime(1970, 1, 1);
}
```

Use `const` for true constants known at compile time; `static readonly` for fixed values computed at runtime.

## Object methods you'll override

Every object inherits these from `object`; override them when your type represents data:

```csharp
public class Point
{
    public int X { get; init; }
    public int Y { get; init; }

    public override string ToString() => $"({X}, {Y})";      // controls how it prints

    public override bool Equals(object? obj)                 // value equality
        => obj is Point p && p.X == X && p.Y == Y;

    public override int GetHashCode() => HashCode.Combine(X, Y);  // for dictionaries/sets
}
```

`ToString()` is what `Console.WriteLine(obj)` and string interpolation use. Overriding `Equals`/`GetHashCode` correctly is fiddly — which is exactly why **`record` types** (Chapter 7) auto-generate all of this for you. For data models, prefer records.

## Partial classes (just so you recognize them)

`partial` lets one class be split across multiple files — used heavily by code generators (e.g., designer files, EF, source generators). You rarely write them by hand.

```csharp
public partial class Customer { /* your code */ }
// ...elsewhere, generated:
public partial class Customer { /* generated code */ }
```

## Exercises

1. Build a `Temperature` class storing Celsius privately, with a `Celsius` property (validating ≥ −273.15) and a computed read-only `Fahrenheit` property.
2. Build a `ShoppingCart` with a private `List<CartItem>`, exposed as `IReadOnlyList`, plus `AddItem`, `RemoveItem`, and a computed `Total`. Don't let callers mutate the list directly.
3. Add a static factory method `BankAccount.Open(decimal initialDeposit)` and a static counter of how many accounts have been opened.
4. Make an immutable `Money` type using `init` properties and `required` for the amount and currency; override `ToString`.

---

**Next:** [06 — Inheritance & Interfaces](./06-inheritance-and-interfaces.md)
