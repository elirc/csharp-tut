# 07 - Structs, Records & Enums Examples

These snippets reinforce enums, flags, records, structs, tuples, and deconstruction.

## Example 1 - Basic enum

```csharp
public enum OrderStatus
{
    Pending,
    Paid,
    Shipped,
    Cancelled
}
```

Enums give names to a fixed set of related values.

## Example 2 - Using an enum

```csharp
OrderStatus status = OrderStatus.Paid;
if (status == OrderStatus.Paid)
{
    Console.WriteLine("Ready to ship");
}
```

Comparing enum values is clearer than comparing magic strings or numbers.

## Example 3 - Custom enum values

```csharp
public enum Priority
{
    Low = 1,
    Medium = 5,
    High = 10
}
```

You can assign underlying numeric values when those values matter outside the program.

## Example 4 - Enum parsing

```csharp
if (Enum.TryParse("High", out Priority priority))
{
    Console.WriteLine(priority);
}
```

`TryParse` converts text to an enum value without throwing for invalid input.

## Example 5 - Flags enum

```csharp
[Flags]
public enum FileAccessOptions
{
    None = 0,
    Read = 1,
    Write = 2,
    Execute = 4
}
```

Flags enums use powers of two so values can be combined.

## Example 6 - Combining flags

```csharp
var access = FileAccessOptions.Read | FileAccessOptions.Write;
bool canWrite = access.HasFlag(FileAccessOptions.Write);
```

The bitwise OR operator combines flags into one value.

## Example 7 - Positional record

```csharp
public record CustomerDto(int Id, string Name);
```

Records are concise data types with value-based equality by default.

## Example 8 - Record equality

```csharp
var a = new CustomerDto(1, "Ada");
var b = new CustomerDto(1, "Ada");
Console.WriteLine(a == b);
```

Two records with the same data compare equal even when they are separate instances.

## Example 9 - With expression

```csharp
var original = new CustomerDto(1, "Ada");
var renamed = original with { Name = "Ada Lovelace" };
```

`with` creates a copy with selected properties changed.

## Example 10 - Record with body

```csharp
public record Invoice(int Id, decimal Subtotal)
{
    public decimal Total => Subtotal * 1.08m;
}
```

Records can include calculated properties and methods just like classes.

## Example 11 - Record class with required properties

```csharp
public record Address
{
    public required string City { get; init; }
    public required string State { get; init; }
}
```

Record bodies are useful when positional parameters would become too long or unclear.

## Example 12 - Record struct

```csharp
public readonly record struct Money(decimal Amount, string Currency);
```

Record structs provide value-type behavior with record conveniences.

## Example 13 - Simple struct

```csharp
public readonly struct Point
{
    public Point(int x, int y) => (X, Y) = (x, y);
    public int X { get; }
    public int Y { get; }
}
```

Structs are value types and work best when small and immutable.

## Example 14 - Mutable struct trap

```csharp
public struct Counter
{
    public int Value;
}
```

Mutable structs can surprise readers because copies can be modified independently.

## Example 15 - Tuple literal

```csharp
var result = (Success: true, Count: 3);
Console.WriteLine(result.Success);
```

Named tuple fields make small grouped values readable.

## Example 16 - Returning a tuple

```csharp
static (int Min, int Max) Bounds(int[] numbers)
{
    return (numbers.Min(), numbers.Max());
}
```

Tuples are practical when returning a few related values from a method.

## Example 17 - Tuple deconstruction

```csharp
var bounds = (Min: 2, Max: 9);
var (min, max) = bounds;
```

Deconstruction assigns tuple elements to separate variables.

## Example 18 - Record deconstruction

```csharp
var customer = new CustomerDto(7, "Grace");
var (id, name) = customer;
```

Positional records automatically support deconstruction in parameter order.

## Example 19 - Discard during deconstruction

```csharp
var (_, customerName) = new CustomerDto(10, "Linus");
```

The discard `_` ignores values you do not need.

## Example 20 - Choosing the shape

```csharp
public enum PaymentKind { Cash, Card }
public record Payment(PaymentKind Kind, decimal Amount);
```

Enums model fixed choices, while records model the data attached to those choices.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `enum Direction { North, South, East, West }` | Models a small fixed set of named choices. |
| 22 | `Direction d = Direction.North;` | Assigns one named enum value to a variable. |
| 23 | `int raw = (int)Direction.North;` | Converts an enum value to its underlying number. |
| 24 | `Direction d = (Direction)2;` | Converts a number back to an enum value. |
| 25 | `Enum.IsDefined(typeof(Direction), d)` | Checks whether a value is declared by the enum. |
| 26 | `Enum.GetNames<Direction>()` | Gets all enum names as strings. |
| 27 | `Enum.GetValues<Direction>()` | Gets all declared values for an enum type. |
| 28 | `[Flags] enum Permissions { None = 0, Read = 1, Write = 2 }` | Marks an enum as a combinable set of options. |
| 29 | `permissions.HasFlag(Permissions.Read)` | Checks whether a combined flags value includes one flag. |
| 30 | `permissions &= ~Permissions.Write;` | Removes a flag from a combined flags value. |
| 31 | `record Person(string Name);` | Declares a concise immutable reference data type. |
| 32 | `var copy = person with { Name = "Grace" };` | Creates a modified copy of a record. |
| 33 | `var same = person1 == person2;` | Records compare by value by default. |
| 34 | `public record User { public required string Email { get; init; } }` | A record body supports required properties. |
| 35 | `public sealed record Token(string Value);` | Prevents inheritance from a record. |
| 36 | `readonly record struct Pixel(byte R, byte G, byte B);` | Creates a small immutable value-type record. |
| 37 | `struct Size { public int Width; public int Height; }` | Declares a custom value type. |
| 38 | `readonly struct Coordinate { }` | Communicates that a struct should not mutate its fields. |
| 39 | `var point = new Point(3, 4);` | Constructs a value using a custom struct or record struct. |
| 40 | `var tuple = (Id: 1, Name: "Ada");` | Creates a lightweight grouped value. |
| 41 | `Console.WriteLine(tuple.Name);` | Reads a named tuple field. |
| 42 | `var (id, name) = tuple;` | Deconstructs a tuple into separate variables. |
| 43 | `var (_, name) = tuple;` | Uses a discard for tuple values you do not need. |
| 44 | `public void Deconstruct(out int x, out int y)` | Enables deconstruction for your own type. |
| 45 | `var (x, y) = point;` | Calls a type's `Deconstruct` method. |
| 46 | `record Range(int Start, int End)` | Records are ideal for simple data transfer shapes. |
| 47 | `class Session { public Guid Id { get; } = Guid.NewGuid(); }` | Classes are better when identity matters. |
| 48 | `record Money(decimal Amount, string Currency)` | Records are good for value-style domain data. |
| 49 | `readonly struct Percentage { public decimal Value { get; } }` | Structs are best when small and immutable. |
| 50 | `var status = OrderStatus.Pending.ToString();` | Converts an enum value to its name. |
| 51 | `Enum.TryParse<OrderStatus>(text, true, out var status)` | Parses enum text while ignoring case. |
| 52 | `record Error(string Code, string Message);` | Records make simple error payloads concise. |
| 53 | `record struct Result(bool Success, int Count);` | Record structs can hold small result data without reference allocation. |
| 54 | `var hash = person.GetHashCode();` | Records generate hash codes from their values. |
| 55 | `Console.WriteLine(person);` | Records generate a helpful `ToString` representation. |
| 56 | `public enum HttpVerb { Get, Post, Put, Delete }` | Enums work well for protocol-like choices. |
| 57 | `var color = (Red: 255, Green: 0, Blue: 0);` | Tuples can name small ad-hoc values. |
| 58 | `static (bool Found, string? Value) Find()` | Tuples make short multi-value returns readable. |
| 59 | `var result = Find(); if (result.Found) { }` | Named tuple fields make call-site code self-explanatory. |
| 60 | `record ValidationError(string Field, string Message);` | A record captures immutable structured data with little ceremony. |
