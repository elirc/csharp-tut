# 05 - Classes & Objects Examples

These examples reinforce fields, properties, constructors, methods, access modifiers, static members, and object overrides.

## Example 1 - Simple class

```csharp
public class Dog
{
    public string Name { get; set; } = "";
}
```

A class defines a reference type with members such as properties and methods.

## Example 2 - Creating an object

```csharp
var dog = new Dog();
dog.Name = "Milo";
```

`new` creates an instance of the class, and the variable stores a reference to it.

## Example 3 - Private field

```csharp
public class Counter
{
    private int _value;
}
```

Fields store object state, and private fields are hidden from outside code.

## Example 4 - Auto-property

```csharp
public class User
{
    public string Email { get; set; } = "";
}
```

Auto-properties are the normal way to expose simple data with getter and setter access.

## Example 5 - Property with validation

```csharp
public class Product
{
    private decimal _price;

    public decimal Price
    {
        get => _price;
        set => _price = value >= 0 ? value : throw new ArgumentOutOfRangeException();
    }
}
```

Full properties let you run logic when values are read or assigned.

## Example 6 - Init-only property

```csharp
public class Book
{
    public string Title { get; init; } = "";
}

var book = new Book { Title = "Clean Code" };
```

`init` allows assignment during object creation but prevents later reassignment.

## Example 7 - Required property

```csharp
public class Customer
{
    public required string Name { get; init; }
}

var customer = new Customer { Name = "Ada" };
```

`required` tells callers they must initialize the property.

## Example 8 - Constructor

```csharp
public class Account
{
    public Account(string owner)
    {
        Owner = owner;
    }

    public string Owner { get; }
}
```

Constructors force important state to be supplied when the object is created.

## Example 9 - Constructor overloads

```csharp
public class Timer
{
    public Timer() : this(60) { }
    public Timer(int seconds) => Seconds = seconds;

    public int Seconds { get; }
}
```

Constructor overloads provide multiple ways to create a valid object.

## Example 10 - Primary constructor

```csharp
public class Person(string firstName, string lastName)
{
    public string FullName => $"{firstName} {lastName}";
}
```

Primary constructors keep simple constructor parameters close to the class declaration.

## Example 11 - Instance method

```csharp
public class BankAccount
{
    public decimal Balance { get; private set; }
    public void Deposit(decimal amount) => Balance += amount;
}
```

Instance methods usually work with the state of one object.

## Example 12 - Encapsulation

```csharp
public class Score
{
    public int Points { get; private set; }
    public void Add(int points) => Points += Math.Max(0, points);
}
```

Private setters prevent outside code from bypassing the class's rules.

## Example 13 - Access modifiers

```csharp
public class Report
{
    public string Title { get; set; } = "";
    private DateTime _createdAt = DateTime.UtcNow;
}
```

`public` members are available to callers, while `private` members are implementation details.

## Example 14 - Static property

```csharp
public class AppInfo
{
    public static string Name => "Tutorial App";
}

Console.WriteLine(AppInfo.Name);
```

Static members belong to the class itself, not to a specific object.

## Example 15 - Static class

```csharp
public static class Temperature
{
    public static double ToCelsius(double f) => (f - 32) * 5 / 9;
}
```

Static classes are useful for stateless helper methods.

## Example 16 - const

```csharp
public class MathSettings
{
    public const int DefaultPrecision = 2;
}
```

`const` values are compile-time constants and cannot depend on runtime values.

## Example 17 - static readonly

```csharp
public class Clock
{
    public static readonly DateTime StartedAt = DateTime.UtcNow;
}
```

`static readonly` values are assigned at runtime and then kept unchanged.

## Example 18 - Override ToString

```csharp
public class Point
{
    public int X { get; init; }
    public int Y { get; init; }
    public override string ToString() => $"({X}, {Y})";
}
```

Overriding `ToString` gives objects a useful display representation.

## Example 19 - Partial class

```csharp
public partial class GeneratedView
{
    public string Title { get; set; } = "";
}
```

Partial classes allow one class definition to be split across multiple files.

## Example 20 - Object initializer

```csharp
var product = new Product
{
    Price = 19.99m
};
```

Object initializers assign public properties immediately after construction.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `public class Invoice { }` | Declares a reference type that can later gain state and behavior. |
| 22 | `var invoice = new Invoice();` | Creates an object instance from a class. |
| 23 | `public string Number { get; set; } = "";` | Adds a read/write auto-property with a safe default. |
| 24 | `public string Number { get; init; } = "";` | Makes a property assignable only during initialization. |
| 25 | `public required string Number { get; init; }` | Forces callers to provide important data. |
| 26 | `private decimal _total;` | Stores internal state that callers cannot access directly. |
| 27 | `public decimal Total => _total;` | Exposes a read-only calculated or backed property. |
| 28 | `public void AddLine(decimal amount) => _total += amount;` | Uses an instance method to change object state. |
| 29 | `public Invoice(string number) => Number = number;` | Initializes required state through a constructor. |
| 30 | `public Invoice() : this("Draft") { }` | Chains constructors to reuse initialization logic. |
| 31 | `public class User(string email)` | Uses a primary constructor for simple required input. |
| 32 | `public string Email { get; } = email;` | Stores a primary constructor parameter in a property. |
| 33 | `private set;` | Allows the class to update a property while callers can only read it. |
| 34 | `protected string Name { get; set; } = "";` | Allows derived classes to access a member. |
| 35 | `internal class Parser { }` | Limits a type to the current assembly. |
| 36 | `public static int Count { get; private set; }` | Tracks shared state across all instances. |
| 37 | `public static readonly Guid AppId = Guid.NewGuid();` | Creates a runtime constant-like value. |
| 38 | `public const string Kind = "Demo";` | Creates a compile-time constant string. |
| 39 | `public override string ToString() => Name;` | Controls how an object displays as text. |
| 40 | `public override bool Equals(object? obj)` | Lets a class define custom equality behavior. |
| 41 | `public override int GetHashCode()` | Keeps hash-based collections consistent with equality. |
| 42 | `public sealed class Token { }` | Prevents other classes from inheriting from this class. |
| 43 | `public abstract class Entity { }` | Creates a base class that cannot be instantiated directly. |
| 44 | `public partial class Customer { }` | Splits one class across multiple files. |
| 45 | `public class Cart { private readonly List<string> _items = new(); }` | Keeps a mutable collection hidden behind the class boundary. |
| 46 | `public IReadOnlyList<string> Items => _items;` | Exposes read-only access to internal collection data. |
| 47 | `public void Clear() => _items.Clear();` | Provides a named operation instead of exposing internals. |
| 48 | `public bool IsEmpty => _items.Count == 0;` | Uses a calculated property for derived state. |
| 49 | `public decimal Balance { get; private set; }` | Protects important state from arbitrary outside assignment. |
| 50 | `public void Withdraw(decimal amount)` | Names a behavior using the language of the domain. |
| 51 | `if (amount <= 0) throw new ArgumentOutOfRangeException(nameof(amount));` | Validates input inside the method that owns the rule. |
| 52 | `private readonly ILogger _logger;` | Stores a dependency as a field. |
| 53 | `public Service(ILogger logger) => _logger = logger;` | Receives dependencies through the constructor. |
| 54 | `public static User Guest() => new User("guest@example.com");` | Uses a static factory for a named creation scenario. |
| 55 | `public DateTime CreatedAt { get; } = DateTime.UtcNow;` | Initializes a read-only property at object creation. |
| 56 | `public string FullName => $"{First} {Last}";` | Calculates display data from other properties. |
| 57 | `public object Clone() => MemberwiseClone();` | Creates a shallow copy from the current object. |
| 58 | `private class Node { }` | Nests a helper class when it is only useful inside another type. |
| 59 | `public class Repository<T> { }` | Classes can be generic too. |
| 60 | `var user = new User { Email = "a@example.com" };` | Object initializers set properties right after construction. |
