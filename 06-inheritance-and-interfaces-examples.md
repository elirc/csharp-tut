# 06 - Inheritance & Interfaces Examples

These snippets reinforce inheritance, polymorphism, abstract classes, interfaces, casting, and composition.

## Example 1 - Basic inheritance

```csharp
public class Animal
{
    public string Name { get; set; } = "";
}

public class Cat : Animal
{
    public bool LikesBoxes { get; set; }
}
```

`Cat` inherits members from `Animal` and can add members of its own.

## Example 2 - Virtual and override

```csharp
public class Shape
{
    public virtual decimal Area() => 0;
}

public class Square : Shape
{
    public decimal Size { get; init; }
    public override decimal Area() => Size * Size;
}
```

`virtual` allows derived classes to replace behavior with `override`.

## Example 3 - Sealed override

```csharp
public class BaseReport
{
    public virtual string Format() => "base";
}

public class FinalReport : BaseReport
{
    public sealed override string Format() => "final";
}
```

`sealed override` prevents further derived classes from overriding that method again.

## Example 4 - Polymorphism

```csharp
Shape shape = new Square { Size = 4 };
Console.WriteLine(shape.Area());
```

The variable type is `Shape`, but the runtime object decides which override runs.

## Example 5 - Calling base

```csharp
public class AuditedReport : BaseReport
{
    public override string Format() => base.Format() + " + audit";
}
```

`base` calls the inherited implementation so you can extend it instead of replacing it fully.

## Example 6 - Constructor chaining

```csharp
public class Employee
{
    public Employee(string name) => Name = name;
    public string Name { get; }
}

public class Manager : Employee
{
    public Manager(string name) : base(name) { }
}
```

A derived constructor can pass required values to the base constructor.

## Example 7 - Abstract class

```csharp
public abstract class Command
{
    public abstract void Execute();
}
```

Abstract classes cannot be instantiated directly and may require derived classes to implement members.

## Example 8 - Implementing an abstract method

```csharp
public class SaveCommand : Command
{
    public override void Execute() => Console.WriteLine("Saved");
}
```

Concrete derived classes must implement abstract members before they can be created.

## Example 9 - Interface declaration

```csharp
public interface INotifier
{
    void Send(string message);
}
```

Interfaces define a contract without committing to a specific implementation.

## Example 10 - Interface implementation

```csharp
public class EmailNotifier : INotifier
{
    public void Send(string message) => Console.WriteLine($"Email: {message}");
}
```

A class that implements an interface promises to provide all required members.

## Example 11 - Depending on an interface

```csharp
static void Alert(INotifier notifier)
{
    notifier.Send("Build finished");
}
```

Accepting an interface makes the method work with any compatible implementation.

## Example 12 - Multiple interfaces

```csharp
public interface IReadable { string Read(); }
public interface IWritable { void Write(string text); }

public class MemoryDocument : IReadable, IWritable
{
    private string _text = "";
    public string Read() => _text;
    public void Write(string text) => _text = text;
}
```

A class can implement multiple interfaces when it supports multiple contracts.

## Example 13 - Common BCL interface

```csharp
public class NameComparer : IComparer<string>
{
    public int Compare(string? x, string? y) => string.Compare(x, y, StringComparison.OrdinalIgnoreCase);
}
```

The base class library uses interfaces heavily to plug custom behavior into existing APIs.

## Example 14 - Default interface member

```csharp
public interface IClock
{
    DateTime Now() => DateTime.UtcNow;
}
```

Default interface members can provide shared behavior, though simple interfaces are often clearer.

## Example 15 - is pattern

```csharp
object value = "hello";
if (value is string text)
{
    Console.WriteLine(text.Length);
}
```

`is` checks the runtime type and can assign a typed variable when the check succeeds.

## Example 16 - as cast

```csharp
object value = "hello";
string? text = value as string;
```

`as` returns null instead of throwing when the object is not compatible with the target type.

## Example 17 - Explicit cast

```csharp
object value = "hello";
string text = (string)value;
```

An explicit cast throws if the runtime object is not actually compatible.

## Example 18 - Composition

```csharp
public class OrderService
{
    private readonly INotifier _notifier;
    public OrderService(INotifier notifier) => _notifier = notifier;
}
```

Composition means building behavior by holding other objects rather than inheriting from them.

## Example 19 - Interface for testing

```csharp
public class FakeNotifier : INotifier
{
    public List<string> Sent { get; } = new();
    public void Send(string message) => Sent.Add(message);
}
```

Interfaces make it easy to swap real dependencies for test doubles.

## Example 20 - Prefer small contracts

```csharp
public interface ICanSave
{
    void Save();
}
```

Small interfaces are easier to implement, test, and reuse than broad catch-all contracts.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `public class Car : Vehicle { }` | A derived class inherits members from its base class. |
| 22 | `public virtual void Start() { }` | Marks a base method as replaceable by derived classes. |
| 23 | `public override void Start() { }` | Replaces inherited virtual behavior. |
| 24 | `public sealed override void Start() { }` | Stops further subclasses from overriding the method again. |
| 25 | `Vehicle vehicle = new Car();` | Stores a derived object in a base-type variable. |
| 26 | `vehicle.Start();` | Calls virtual behavior through the base-type reference. |
| 27 | `base.Start();` | Invokes the base implementation from an override. |
| 28 | `public Truck(string vin) : base(vin) { }` | Passes constructor data up to the base class. |
| 29 | `public abstract decimal Area();` | Requires derived classes to provide an implementation. |
| 30 | `public interface IPrintable { void Print(); }` | Defines a behavior contract. |
| 31 | `public class Receipt : IPrintable { public void Print() { } }` | Implements an interface member. |
| 32 | `static void Print(IPrintable item) => item.Print();` | Depends on an interface instead of a concrete type. |
| 33 | `public interface IRepository<T> { T? Find(int id); }` | Interfaces can be generic. |
| 34 | `public interface IReadOnlyThing { string Name { get; } }` | Interfaces can require properties. |
| 35 | `public interface IAsyncLoader { Task LoadAsync(); }` | Interfaces often describe asynchronous capabilities. |
| 36 | `if (obj is Car car) car.Start();` | Pattern matching checks and casts in one step. |
| 37 | `Car? car = obj as Car;` | `as` returns null when the cast cannot work. |
| 38 | `var car = (Car)obj;` | An explicit cast throws if the runtime type is wrong. |
| 39 | `public class OrderService(INotifier notifier)` | Composition injects a collaborator instead of inheriting behavior. |
| 40 | `private readonly INotifier _notifier = notifier;` | Stores a composed dependency for later use. |
| 41 | `public interface IClock { DateTime Now(); }` | Small interfaces make dependencies easy to replace. |
| 42 | `public class SystemClock : IClock { public DateTime Now() => DateTime.UtcNow; }` | Provides a real implementation behind a small interface. |
| 43 | `public class FakeClock : IClock { public DateTime Now() => Fixed; }` | Test implementations can make behavior predictable. |
| 44 | `public abstract class Animal { public abstract string Speak(); }` | Abstract classes can define required behavior plus shared state. |
| 45 | `public class Dog : Animal { public override string Speak() => "woof"; }` | Derived classes satisfy abstract requirements. |
| 46 | `public interface IDisposable { void Dispose(); }` | A familiar interface describes cleanup behavior. |
| 47 | `public class FileStore : IDisposable` | Implementing cleanup contracts lets callers use `using`. |
| 48 | `public interface IHandler<in T> { void Handle(T value); }` | `in` marks a contravariant type parameter. |
| 49 | `public interface IProvider<out T> { T Get(); }` | `out` marks a covariant type parameter. |
| 50 | `public class AdminUser : User { }` | Inheritance can model a true is-a relationship. |
| 51 | `public class User { public Role Role { get; init; } }` | Composition can model roles without subclass explosion. |
| 52 | `public interface IValidator<T> { bool IsValid(T value); }` | Generic interfaces define reusable contracts. |
| 53 | `public class EmailValidator : IValidator<string>` | Implements a generic interface for one concrete type. |
| 54 | `IEnumerable<string> names = new List<string>();` | Program to an interface when the implementation is not important. |
| 55 | `IReadOnlyList<string> names = new List<string>();` | Choose an interface that exposes exactly what callers need. |
| 56 | `public virtual string Describe() => ToString() ?? "";` | Virtual methods can provide a default implementation. |
| 57 | `protected virtual void OnSaved() { }` | Protected virtual hooks let subclasses participate in workflows. |
| 58 | `public interface ICommand { void Execute(); }` | Command interfaces are useful for plug-in behavior. |
| 59 | `List<ICommand> commands = new();` | A collection of interfaces can hold many implementations. |
| 60 | `foreach (var command in commands) command.Execute();` | Polymorphism lets each object respond in its own way. |
