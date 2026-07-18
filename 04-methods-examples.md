# 04 - Methods Examples

These snippets reinforce method anatomy, parameters, return values, overloads, local functions, recursion, and design habits.

## Example 1 - A void method

```csharp
static void SayHello()
{
    Console.WriteLine("Hello");
}
```

`void` means the method performs work but does not return a value.

## Example 2 - Returning a value

```csharp
static int Add(int a, int b)
{
    return a + b;
}
```

The return type tells callers what kind of value the method produces.

## Example 3 - Expression-bodied method

```csharp
static int Square(int number) => number * number;
```

Expression-bodied syntax is concise when a method is just one expression.

## Example 4 - Parameter order matters

```csharp
static decimal ApplyDiscount(decimal price, decimal percent)
{
    return price * (1 - percent);
}
```

Parameters define the values callers must supply and the order they normally appear in.

## Example 5 - Arguments passed by value

```csharp
static void Increment(int value)
{
    value++;
}

int count = 1;
Increment(count);
Console.WriteLine(count);
```

The method receives a copy of the integer, so the original variable remains unchanged.

## Example 6 - ref parameter

```csharp
static void Increment(ref int value)
{
    value++;
}

int count = 1;
Increment(ref count);
```

`ref` lets a method modify the caller's variable directly.

## Example 7 - out parameter

```csharp
static bool TryGetLength(string? text, out int length)
{
    length = text?.Length ?? 0;
    return text is not null;
}
```

`out` lets a method return an extra value alongside its main result.

## Example 8 - in parameter

```csharp
static decimal Total(in decimal subtotal, in decimal tax)
{
    return subtotal + tax;
}
```

`in` passes by reference but prevents the method from assigning a new value to the parameter.

## Example 9 - params array

```csharp
static int Sum(params int[] numbers)
{
    return numbers.Sum();
}

Console.WriteLine(Sum(1, 2, 3));
```

`params` lets callers pass any number of arguments of the same type.

## Example 10 - Optional parameter

```csharp
static void Log(string message, string level = "Info")
{
    Console.WriteLine($"[{level}] {message}");
}
```

Optional parameters provide a default value when the caller leaves an argument out.

## Example 11 - Named arguments

```csharp
static void Resize(int width, int height)
{
    Console.WriteLine($"{width}x{height}");
}

Resize(height: 1080, width: 1920);
```

Named arguments can make calls clearer and allow arguments to appear out of order.

## Example 12 - Overloading

```csharp
static int Double(int value) => value * 2;
static decimal Double(decimal value) => value * 2;
```

Overloads share a name but differ by parameter types or counts.

## Example 13 - Local function

```csharp
static bool IsStrongPassword(string password)
{
    bool HasDigit() => password.Any(char.IsDigit);
    return password.Length >= 8 && HasDigit();
}
```

Local functions keep helper logic close to the method that uses it.

## Example 14 - Recursion

```csharp
static int Factorial(int n)
{
    if (n <= 1) return 1;
    return n * Factorial(n - 1);
}
```

Recursive methods call themselves and need a base case to stop.

## Example 15 - Static method call

```csharp
static double CircleArea(double radius)
{
    return Math.PI * radius * radius;
}
```

Static methods belong to the type rather than a particular object instance.

## Example 16 - Instance method preview

```csharp
var name = "ada";
string title = name.ToUpper();
```

Instance methods run on a specific object, such as this string value.

## Example 17 - Guard clause

```csharp
static decimal Divide(decimal numerator, decimal denominator)
{
    if (denominator == 0) throw new ArgumentException("Cannot divide by zero.");
    return numerator / denominator;
}
```

A guard clause rejects invalid input before the main method logic runs.

## Example 18 - Small method, clear name

```csharp
static bool IsWeekend(DayOfWeek day)
{
    return day is DayOfWeek.Saturday or DayOfWeek.Sunday;
}
```

Good method names let callers understand intent without reading the implementation.

## Example 19 - Method composition

```csharp
static decimal AddTax(decimal subtotal) => subtotal * 1.08m;
static decimal AddShipping(decimal total) => total + 5m;

decimal final = AddShipping(AddTax(100m));
```

Small methods can be combined to express a larger workflow.

## Example 20 - Returning a tuple

```csharp
static (int Min, int Max) GetRange(int[] numbers)
{
    return (numbers.Min(), numbers.Max());
}
```

Tuples are a lightweight way to return multiple related values.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `static void PrintLine() => Console.WriteLine();` | A tiny void method can wrap a repeated action. |
| 22 | `static string Greet(string name) => $"Hi, {name}";` | Returns a string built from a parameter. |
| 23 | `static bool IsEven(int n) => n % 2 == 0;` | Names a common boolean test. |
| 24 | `static decimal AddTax(decimal subtotal, decimal rate) => subtotal * (1 + rate);` | Keeps a calculation reusable and testable. |
| 25 | `static void Rename(ref string name) => name = name.Trim();` | Uses `ref` when the caller's variable must be reassigned. |
| 26 | `static bool TryDivide(decimal a, decimal b, out decimal result)` | A Try-style signature avoids exceptions for expected failure. |
| 27 | `static int Sum(params int[] values) => values.Sum();` | Uses `params` for a flexible number of inputs. |
| 28 | `static void Connect(string host, int port = 80) { }` | Optional parameters work well for common defaults. |
| 29 | `Connect(port: 443, host: "example.com");` | Named arguments make call-site meaning explicit. |
| 30 | `static int ParseOrDefault(string text, int fallback = 0)` | Combines parsing logic with a default fallback. |
| 31 | `static int Area(int side) => side * side;` | One overload can handle square area. |
| 32 | `static int Area(int width, int height) => width * height;` | Another overload handles rectangular area. |
| 33 | `static T Echo<T>(T value) => value;` | Generic methods work with a caller-chosen type. |
| 34 | `static void ThrowIfNegative(int value)` | A validation helper can make calling code cleaner. |
| 35 | `static string Normalize(string value) => value.Trim().ToUpperInvariant();` | Methods can hide noisy transformation details. |
| 36 | `static IEnumerable<int> Range(int max) { for (int i = 0; i < max; i++) yield return i; }` | Iterator methods produce sequences lazily. |
| 37 | `static int Fib(int n) => n <= 1 ? n : Fib(n - 1) + Fib(n - 2);` | Recursion expresses problems defined in terms of smaller versions. |
| 38 | `static void Log(string message, DateTime? at = null)` | Nullable optional parameters can mean use a current value. |
| 39 | `static double Average(IReadOnlyList<int> values)` | Interfaces make parameters more flexible. |
| 40 | `static void Mutate(List<int> values) => values.Add(1);` | Passing a reference type by value still lets you mutate the object. |
| 41 | `static void Replace(List<int> values) => values = new();` | Reassigning a by-value parameter does not replace the caller's variable. |
| 42 | `static void Replace(ref List<int> values) => values = new();` | `ref` allows replacement of the caller's variable. |
| 43 | `static (bool Ok, int Value) TryRead()` | Tuples can return a status and data together. |
| 44 | `static void PrintAll(IEnumerable<string> lines)` | Accept broad interfaces when you only enumerate values. |
| 45 | `static string Join(string separator, params string[] parts)` | Combines a normal parameter with `params`. |
| 46 | `static int Clamp(int value, int min, int max)` | Small utility methods can capture business rules. |
| 47 | `static bool Between(int value, int min, int max) => value >= min && value <= max;` | A method name can make a compound condition easier to read. |
| 48 | `static void Save(string path, string contents)` | Use parameter names that identify roles clearly. |
| 49 | `static Stream OpenRead(string path)` | Return abstractions when callers do not need a concrete type. |
| 50 | `static async Task SaveAsync(string path, string text)` | Async methods should return `Task` or `Task<T>`. |
| 51 | `static void Configure(Action<Options> configure)` | Methods can accept behavior as a parameter. |
| 52 | `static TResult Use<T, TResult>(T input, Func<T, TResult> map)` | Generic delegate parameters create flexible APIs. |
| 53 | `static int CountWhere<T>(IEnumerable<T> items, Predicate<T> test)` | A predicate parameter lets callers define the condition. |
| 54 | `static void Print<T>(T value) where T : notnull` | Constraints communicate what a generic method requires. |
| 55 | `static T Create<T>() where T : new()` | Constructor constraints allow `new T()`. |
| 56 | `static decimal Total(params decimal[] amounts)` | A variadic total method can accept any count of amounts. |
| 57 | `static bool HasText(string? text) => !string.IsNullOrWhiteSpace(text);` | A tiny method can centralize a repeated null check. |
| 58 | `static void Require(bool condition, string message)` | Guard helpers can standardize validation behavior. |
| 59 | `static string Describe(object value) => value.ToString() ?? "";` | Return types should avoid null when callers expect text. |
| 60 | `static void Main(string[] args)` | The classic entry point can receive command-line arguments. |
