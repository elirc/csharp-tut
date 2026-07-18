# 14 - Pattern Matching & Modern Syntax Examples

These snippets reinforce type patterns, property patterns, tuple patterns, list patterns, modern object creation, ranges, `nameof`, and null operators.

## Example 1 - Type pattern

```csharp
object value = "hello";
if (value is string text)
{
    Console.WriteLine(text.Length);
}
```

The type pattern checks the runtime type and creates a strongly typed variable.

## Example 2 - Null pattern

```csharp
if (customer is null)
{
    Console.WriteLine("No customer loaded.");
}
```

`is null` avoids overloaded equality operators and reads clearly.

## Example 3 - Not null pattern

```csharp
if (customer is not null)
{
    Console.WriteLine(customer.Name);
}
```

`is not null` narrows the variable so members can be accessed safely.

## Example 4 - Constant pattern

```csharp
string command = "quit";
if (command is "quit")
{
    Console.WriteLine("Exiting");
}
```

Constant patterns compare a value to a specific constant.

## Example 5 - Relational pattern

```csharp
string label = score switch
{
    >= 90 => "Excellent",
    >= 70 => "Pass",
    _ => "Retry"
};
```

Relational patterns make threshold checks compact.

## Example 6 - Logical pattern

```csharp
bool isTeen = age is >= 13 and <= 19;
```

`and`, `or`, and `not` combine patterns without repeating the variable.

## Example 7 - Switch expression

```csharp
string message = status switch
{
    OrderStatus.Pending => "Waiting",
    OrderStatus.Shipped => "On the way",
    _ => "Unknown"
};
```

Switch expressions are best when each case produces a value.

## Example 8 - Property pattern

```csharp
if (order is { Total: > 100m, IsPaid: true })
{
    Console.WriteLine("Priority fulfillment");
}
```

Property patterns match based on an object's property values.

## Example 9 - Nested property pattern

```csharp
if (order is { Customer.Address.State: "CA" })
{
    Console.WriteLine("California order");
}
```

Nested property patterns inspect data deeper in an object graph.

## Example 10 - Tuple pattern

```csharp
string access = (role, isActive) switch
{
    ("Admin", true) => "Full",
    (_, false) => "Disabled",
    _ => "Standard"
};
```

Tuple patterns match combinations of values without nesting several `if` statements.

## Example 11 - When guard

```csharp
string size = order switch
{
    { Total: > 0 } when order.Items.Count > 10 => "Bulk",
    { Total: > 0 } => "Normal",
    _ => "Invalid"
};
```

`when` guards add extra conditions to a pattern arm.

## Example 12 - List pattern

```csharp
int[] numbers = { 1, 2, 3 };
bool startsWithOne = numbers is [1, ..];
```

List patterns match the shape and contents of a sequence.

## Example 13 - List pattern with discard

```csharp
bool hasThreeItems = numbers is [_, _, _];
```

Discards match any item when only the sequence shape matters.

## Example 14 - Positional pattern

```csharp
var point = new Point(0, 5);
string location = point switch
{
    (0, 0) => "Origin",
    (0, _) => "Y axis",
    _ => "Other"
};
```

Positional patterns match values exposed by deconstruction.

## Example 15 - Target-typed new

```csharp
List<string> names = new();
```

When the type is clear from the left side, `new()` keeps object creation shorter.

## Example 16 - Collection expression

```csharp
int[] values = [1, 2, 3];
```

Collection expressions are a modern concise syntax for creating collections.

## Example 17 - Range

```csharp
string text = "abcdef";
string middle = text[1..4];
```

Ranges select a slice using a start index and an exclusive end index.

## Example 18 - Index from end

```csharp
char last = text[^1];
```

The `^` operator indexes from the end of a sequence.

## Example 19 - nameof

```csharp
throw new ArgumentException("Required.", nameof(customer));
```

`nameof` keeps parameter and member names safe during refactoring.

## Example 20 - Null operators together

```csharp
int itemCount = order?.Items?.Count ?? 0;
```

`?.` safely navigates nullable references, and `??` supplies a fallback.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `value is int number` | Checks type and introduces a typed variable. |
| 22 | `value is not string` | Matches anything that is not a string. |
| 23 | `text is { Length: > 3 }` | Checks that text is non-null and longer than three characters. |
| 24 | `order is { Total: <= 0 }` | Matches an object with a property in a range. |
| 25 | `customer is { Address: not null }` | Checks a nested object reference. |
| 26 | `customer is { Address.City: "Seattle" }` | Matches a nested property value. |
| 27 | `shape is Circle { Radius: > 0 }` | Combines a type pattern and property pattern. |
| 28 | `n is > 0 and < 100` | Matches a number inside an exclusive range. |
| 29 | `day is Saturday or Sunday` | Matches one of two constant values. |
| 30 | `name is not null and not ""` | Combines null and constant checks. |
| 31 | `items is []` | Matches an empty collection. |
| 32 | `items is [var first, ..]` | Captures the first item from a non-empty collection. |
| 33 | `items is [.., var last]` | Captures the last item from a non-empty collection. |
| 34 | `items is [1, 2, 3]` | Matches an exact list shape and values. |
| 35 | `items is [1, .. var rest]` | Matches a first value and captures the remaining items. |
| 36 | `(x, y) is (0, 0)` | Matches a tuple against exact values. |
| 37 | `(role, active) is ("Admin", true)` | Matches a combination of values together. |
| 38 | `point is (0, _)` | Uses a positional pattern with a discard. |
| 39 | `command switch { "start" => Start(), _ => Help() }` | Maps command text to behavior. |
| 40 | `status switch { null => "missing", _ => "present" }` | Uses a switch expression to handle null. |
| 41 | `obj switch { string s => s.Length, int n => n, _ => 0 }` | Switches by runtime type. |
| 42 | `order switch { { Total: > 100 } => "large", _ => "normal" }` | Switches by object property. |
| 43 | `case string s when s.Length > 10:` | Adds a guard to a switch statement case. |
| 44 | `List<int> values = new();` | Uses target-typed `new` to reduce repetition. |
| 45 | `Dictionary<string, int> counts = new();` | Target-typed `new` works with generic types too. |
| 46 | `int[] numbers = [1, 2, 3];` | Uses a collection expression for an array. |
| 47 | `List<string> names = ["Ada", "Grace"];` | Uses a collection expression for a list. |
| 48 | `int[] combined = [..left, ..right];` | Spreads existing collections into a new collection expression. |
| 49 | `var firstThree = values[..3];` | Takes items from the start through an exclusive end. |
| 50 | `var afterFirst = values[1..];` | Takes items from an index to the end. |
| 51 | `var lastTwo = values[^2..];` | Takes a range relative to the end. |
| 52 | `nameof(order.Total)` | Produces a member name that refactors safely. |
| 53 | `message ??= "default";` | Assigns a fallback only when the variable is null. |
| 54 | `customer?.Address?.City` | Safely navigates nullable object chains. |
| 55 | `public string Name { get; init; }` | Allows object-initializer assignment only during creation. |
| 56 | `public required string Email { get; init; }` | Requires callers to initialize a property. |
| 57 | `public class User(string email)` | Uses a primary constructor. |
| 58 | `public int Count => _items.Count;` | Uses an expression-bodied property. |
| 59 | `_ = int.TryParse(text, out var number);` | Uses a discard for an ignored return value. |
| 60 | `var raw = $$"""Value: {{amount}}""";` | Raw interpolated strings help with text containing braces. |
