# 14 — Pattern Matching & Modern Syntax

C# has accumulated a lot of expressive, concise syntax over recent versions. This chapter collects the modern features you'll see in any current codebase: pattern matching, switch expressions, and a grab-bag of quality-of-life syntax. None of it is essential to *function*, but reading modern code requires knowing it, and using it makes your code cleaner.

## Pattern matching with `is`

The `is` operator tests a value against a **pattern** and can bind a variable when it matches.

### Type pattern

```csharp
object obj = GetSomething();

if (obj is string s)              // is it a string? if so, bind it to s
    Console.WriteLine(s.Length);

if (obj is int n && n > 0)        // type + extra condition
    Console.WriteLine(n);
```

### Constant and null patterns

```csharp
if (status is OrderStatus.Cancelled) { }
if (value is null) { }            // preferred null check
if (value is not null) { }        // negated
if (x is 0 or 1) { }              // 'or' pattern
```

`is null` / `is not null` are the idiomatic null checks — they can't be fooled by overloaded `==` operators, unlike `== null`.

### Relational and logical patterns

```csharp
string Size(int n) => n switch
{
    < 0 => "negative",
    0 => "zero",
    > 0 and < 10 => "small",
    >= 10 and < 100 => "medium",
    _ => "large"
};

bool IsLetter(char c) => c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z');
```

Patterns combine with `and`, `or`, `not`, and relational operators (`<`, `>`, `<=`, `>=`).

## Switch expressions (recap + patterns)

The switch *expression* (introduced in Chapter 3) shines with patterns. It returns a value and the compiler warns if cases aren't exhaustive:

```csharp
decimal Discount(Customer c) => c switch
{
    { IsVip: true } => 0.20m,                    // property pattern
    { OrderCount: > 10 } => 0.10m,
    { OrderCount: > 0 } => 0.05m,
    _ => 0m
};
```

### Property patterns

Match on an object's properties (and nest them):

```csharp
string Describe(Order o) => o switch
{
    { Status: OrderStatus.Cancelled } => "Cancelled",
    { Total: > 1000, Customer.IsVip: true } => "Big VIP order",   // nested property
    { Total: > 1000 } => "Big order",
    _ => "Standard order"
};
```

### Tuple patterns

Switch on multiple values at once:

```csharp
string RockPaperScissors(string a, string b) => (a, b) switch
{
    ("rock", "scissors") or ("scissors", "paper") or ("paper", "rock") => "Player 1 wins",
    (_, _) when a == b => "Tie",
    _ => "Player 2 wins"
};
```

### `when` guards

Add arbitrary conditions to a case:

```csharp
var result = shape switch
{
    Circle c when c.Radius > 100 => "huge circle",
    Circle => "circle",
    Square => "square",
    _ => "unknown"
};
```

### List patterns (C# 11+)

Match the shape and contents of a sequence:

```csharp
int[] arr = { 1, 2, 3 };
var msg = arr switch
{
    [] => "empty",
    [var single] => $"one element: {single}",
    [var first, .., var last] => $"starts {first}, ends {last}",  // .. = any middle
    _ => "other"
};
```

## Recursive/positional patterns with records

Records' deconstruction enables clean positional matching:

```csharp
public record Point(int X, int Y);

string Quadrant(Point p) => p switch
{
    (0, 0) => "origin",
    (> 0, > 0) => "Q1",
    (< 0, > 0) => "Q2",
    var (x, y) when x == y => "on diagonal",
    _ => "somewhere"
};
```

## Other modern syntax you'll encounter

### Target-typed `new`

When the type is known from the left side, omit it on the right:

```csharp
List<Customer> customers = new();
Dictionary<string, List<int>> map = new();
Point p = new(3, 4);
private readonly HttpClient _http = new();
```

### Collection expressions (C# 12+)

```csharp
int[] a = [1, 2, 3];
List<string> names = ["Ann", "Bob"];
int[] combined = [..a, 4, 5];          // spread
Span<int> span = [1, 2, 3];
```

### Range and index (recap)

```csharp
var slice = list[1..^1];   // from index 1 to one-before-end
var last = list[^1];       // last element
```

### `nameof`

Compile-time name as a string — refactor-safe, used in exceptions, logging, validation:

```csharp
throw new ArgumentNullException(nameof(customer));
_logger.LogInformation("Updating {Property}", nameof(Customer.Email));
```

### Null-related operators (recap, all in one place)

```csharp
x ?? fallback        // null-coalescing: use fallback if x is null
x ??= fallback       // assign fallback only if x is null
obj?.Member          // null-conditional: null if obj is null
arr?[0]              // null-conditional indexing
obj?.Method()        // call only if non-null
x!                   // null-forgiving: "trust me, not null" (Chapter 15)
```

### Expression-bodied members (recap)

```csharp
public string FullName => $"{First} {Last}";
public int Square(int x) => x * x;
public override string ToString() => Name;
```

### String features

```csharp
$"interpolated {value}"            // interpolation
$"{price:C}"                       // with format specifier
@"C:\literal\path"                 // verbatim (no escapes)
"""raw string literal"""           // raw (C# 11+), great for JSON/SQL
$$"""raw {{interpolated}}"""        // raw + interpolation
```

### Discards `_`

Ignore values you don't need:

```csharp
_ = SomeMethodReturningSomethingIgnore();
var (_, year) = GetMonthAndYear();          // ignore month
if (int.TryParse(s, out _)) { }             // only care that it parsed
```

### Local functions, primary constructors, `init`, `required`

Covered in earlier chapters — all part of modern idiom. You'll see primary constructors especially in DI-heavy classes:

```csharp
public class OrderService(IOrderRepository repo, ILogger<OrderService> logger)
{
    public async Task PlaceAsync(Order o)
    {
        logger.LogInformation("Placing order");
        await repo.AddAsync(o);
    }
}
```

## When pattern matching beats if/else chains

Reach for switch expressions / pattern matching when you're mapping an input (its type, shape, or property values) to an output value, or branching on several conditions. They're more concise, exhaustiveness-checked, and side-effect-free. Keep using plain `if` for imperative, step-by-step logic and guard clauses.

```csharp
// Pattern matching: clean mapping
public decimal ShippingCost(Parcel p) => p switch
{
    { Weight: <= 1 } => 5m,
    { Weight: <= 5, IsExpress: false } => 10m,
    { Weight: <= 5, IsExpress: true } => 20m,
    _ => 50m
};
```

## Exercises

1. Rewrite an HTTP-status-to-message mapping as a switch expression with relational patterns (2xx/3xx/4xx/5xx).
2. Write a `Describe(object value)` using a type-pattern switch handling `int`, `string`, `null`, `IEnumerable<int>` (empty vs non-empty via list pattern), and a fallback.
3. Use a property pattern to compute a loyalty tier from a `Customer` record's `OrderCount` and `TotalSpend`.
4. Use a tuple pattern to implement a small state machine: given `(currentState, event)`, return the next state.

---

**Next:** [15 — Nullable Reference Types](./15-nullable-reference-types.md)
