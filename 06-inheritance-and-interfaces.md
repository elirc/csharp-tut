# 06 — Inheritance & Interfaces

This chapter covers the polymorphism tools: inheritance, `virtual`/`override`, abstract classes, and interfaces. In real CRUD code you'll use **interfaces** heavily (for dependency injection and testing) and **inheritance** more sparingly. Both matter; know when to reach for each.

## Inheritance basics

A class can **inherit** from one base class, gaining its members and adding/overriding behavior. Use `:` to specify the base.

```csharp
public class Animal
{
    public string Name { get; set; } = "";

    public void Eat() => Console.WriteLine($"{Name} is eating");

    public virtual void Speak() => Console.WriteLine("Some sound");  // can be overridden
}

public class Dog : Animal      // Dog "is an" Animal
{
    public override void Speak() => Console.WriteLine("Woof");       // override base
}

public class Cat : Animal
{
    public override void Speak() => Console.WriteLine("Meow");
}

Animal a = new Dog { Name = "Rex" };
a.Eat();      // "Rex is eating"  (inherited)
a.Speak();    // "Woof"           (Dog's override runs — this is polymorphism)
```

C# is **single inheritance** for classes — one base class only. (But you can implement many interfaces.)

### `virtual` / `override` / `sealed`

- `virtual` — the base marks a method as overridable.
- `override` — the derived class replaces it.
- `sealed override` — override it but forbid further overriding down the chain.
- `sealed class` — the class can't be inherited at all.

```csharp
public sealed class Poodle : Dog       // nobody can inherit from Poodle
{
    public sealed override void Speak() => Console.WriteLine("Yip");
}
```

If a method is **not** `virtual`, a derived class can't truly override it (it can `new`-hide it, which is almost always a mistake — avoid `new` as a method modifier).

### Polymorphism — the point of all this

A variable typed as the base can hold any derived object, and calls dispatch to the actual type's override at runtime:

```csharp
List<Animal> zoo = new() { new Dog { Name = "Rex" }, new Cat { Name = "Mia" } };
foreach (Animal animal in zoo)
    animal.Speak();      // "Woof", then "Meow" — each runs its own override
```

You write code against the general type (`Animal`) and it works for all specific types. This is *the* OOP superpower.

### Calling the base implementation

```csharp
public class LoudDog : Dog
{
    public override void Speak()
    {
        base.Speak();                      // call Dog's version
        Console.WriteLine("(loudly)");
    }
}
```

### Constructors and inheritance

Derived constructors must ensure the base is initialized, via `: base(...)`:

```csharp
public class Animal
{
    public string Name { get; }
    public Animal(string name) => Name = name;
}

public class Dog : Animal
{
    public string Breed { get; }
    public Dog(string name, string breed) : base(name)   // pass to base ctor
    {
        Breed = breed;
    }
}
```

## Abstract classes

An **abstract class** can't be instantiated; it's a partial base meant to be inherited. It can have abstract members (no body — subclasses must implement) and concrete members (shared implementation).

```csharp
public abstract class Shape
{
    public abstract double Area();           // no body — each shape must define it
    public abstract double Perimeter();

    public void Describe()                    // concrete, shared by all shapes
        => Console.WriteLine($"Area={Area():F2}, Perimeter={Perimeter():F2}");
}

public class Circle : Shape
{
    public double Radius { get; init; }
    public override double Area() => Math.PI * Radius * Radius;
    public override double Perimeter() => 2 * Math.PI * Radius;
}

public class Square : Shape
{
    public double Side { get; init; }
    public override double Area() => Side * Side;
    public override double Perimeter() => 4 * Side;
}

// Shape s = new Shape();          // error: can't instantiate abstract class
Shape s = new Circle { Radius = 2 };
s.Describe();                       // uses the shared method + Circle's overrides
```

Use an abstract class when subclasses share **state and/or implementation** and there's a real "is-a" relationship.

## Interfaces — the workhorse

An **interface** is a pure contract: a set of members a type promises to provide, with no state. A class can implement **many** interfaces. This is how you achieve flexibility, testability, and dependency injection.

```csharp
public interface IShape
{
    double Area();              // members are implicitly public; no bodies
    double Perimeter();
    string Name { get; }        // interfaces can declare properties too
}

public class Circle : IShape
{
    public double Radius { get; init; }
    public double Area() => Math.PI * Radius * Radius;
    public double Perimeter() => 2 * Math.PI * Radius;
    public string Name => "Circle";
}
```

Convention: interface names start with `I` (`IShape`, `IRepository`, `ILogger`).

### Why interfaces matter so much in real code

You program against the interface, not the concrete class. This lets you swap implementations — including fake ones in tests:

```csharp
public interface INotifier
{
    void Send(string to, string message);
}

public class EmailNotifier : INotifier
{
    public void Send(string to, string message)
        => Console.WriteLine($"Email to {to}: {message}");
}

public class SmsNotifier : INotifier
{
    public void Send(string to, string message)
        => Console.WriteLine($"SMS to {to}: {message}");
}

// Code depends on the interface, not a concrete type:
public class OrderService
{
    private readonly INotifier _notifier;
    public OrderService(INotifier notifier) => _notifier = notifier;

    public void PlaceOrder(string customerEmail)
    {
        // ...save the order...
        _notifier.Send(customerEmail, "Your order is confirmed!");
    }
}

// Pass whichever implementation you want — real or a test double:
var service = new OrderService(new EmailNotifier());
```

This is **dependency injection** (Chapter 18) and it's everywhere in C#. The interface is the seam that makes code testable and swappable. When you write tests (Chapter 21), you pass a fake `INotifier` so no real email is sent.

### Interfaces you'll use from the BCL

You'll *implement* and *consume* these constantly:

- `IEnumerable<T>` — "can be foreach'd." Most collections implement it; LINQ extends it.
- `ICollection<T>`, `IList<T>` — add/remove/index.
- `IReadOnlyList<T>`, `IReadOnlyCollection<T>` — read-only views.
- `IDictionary<TKey,TValue>` — key/value lookups.
- `IComparable<T>` — defines sort order (`CompareTo`).
- `IEquatable<T>` — defines equality.
- `IDisposable` — releases resources (Chapter 11).

```csharp
public class Temperature : IComparable<Temperature>
{
    public double Celsius { get; init; }
    public int CompareTo(Temperature? other)
        => Celsius.CompareTo(other?.Celsius ?? double.MinValue);
}

var temps = new List<Temperature> { new() { Celsius = 20 }, new() { Celsius = 5 } };
temps.Sort();   // uses CompareTo -> sorted ascending
```

### Default interface methods (know they exist)

Since C# 8, interfaces can provide default method bodies. Useful for evolving an interface without breaking implementers, but used sparingly. Don't reach for them early; prefer abstract classes for shared behavior.

## Abstract class vs interface — which to use?

| | Interface | Abstract class |
|---|---|---|
| State (fields) | No | Yes |
| Multiple inheritance | Yes (implement many) | No (one base) |
| Shared implementation | Limited (default methods) | Yes |
| Use when | Defining a capability/contract; need DI/testability | Sharing real code + state among a family of related types |

**Default to interfaces** for contracts and dependencies. Use an abstract base class when concrete subclasses genuinely share state and logic. You can combine them: an interface plus an abstract base that implements it.

## `is`, `as`, and casting between types

```csharp
Animal a = new Dog { Name = "Rex" };

// 'is' with pattern: test type and bind a variable
if (a is Dog dog)
    Console.WriteLine(dog.Breed);

// 'as': cast or null (no exception) — only for reference/nullable types
Dog? maybe = a as Dog;
if (maybe is not null) { /* ... */ }

// Direct cast: throws InvalidCastException if wrong
Dog d = (Dog)a;        // ok here because a really is a Dog
```

Prefer the `is` pattern; it's the cleanest and safest. More patterns in Chapter 14.

## Composition over inheritance (mid-level wisdom)

Deep inheritance hierarchies become rigid and confusing. A widely-held principle: **prefer composition** (an object *has* a collaborator) over inheritance (an object *is* a subtype) unless there's a genuine "is-a" relationship and shared behavior.

```csharp
// Instead of inheriting an Engine, a Car HAS an engine:
public class Car
{
    private readonly IEngine _engine;       // composed dependency
    public Car(IEngine engine) => _engine = engine;
    public void Start() => _engine.Start();
}
```

In typical CRUD/business code you'll define lots of interfaces and inject implementations, and use inheritance mainly for things like a shared base controller or a base entity class — not 5-level deep trees.

## Exercises

1. Define `IPaymentMethod` with `bool Charge(decimal amount)`. Implement `CreditCard` and `Wallet`. Write a `Checkout(IPaymentMethod method, decimal amount)` that uses it.
2. Build an abstract `Employee` with abstract `decimal CalculatePay()` and a concrete `Describe()`. Implement `SalariedEmployee` and `HourlyEmployee`.
3. Make a `Notification` system with `INotifier` and two implementations; loop over a `List<INotifier>` and send to all of them.
4. Implement `IComparable<T>` on a `Version` class (major/minor) and sort a list of them.

---

**Next:** [07 — Structs, Records & Enums](./07-structs-records-enums.md)
