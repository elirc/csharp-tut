# 02 - Types & Variables Examples

These examples reinforce value types, reference types, strings, nullability, conversion, formatting, and boxing.

## Example 1 - Integer types

```csharp
byte small = 255;
int normal = 42;
long huge = 9_000_000_000;
```

Different integer types support different ranges, with `int` being the everyday default.

## Example 2 - Floating point

```csharp
float f = 1.5f;
double d = 1.5;
decimal money = 1.50m;
```

Use `decimal` for money, while `double` is common for measurements and scientific values.

## Example 3 - Digit separators

```csharp
int population = 39_000_000;
decimal price = 1_299.99m;
```

Underscores make large numeric literals easier to read without changing their value.

## Example 4 - Characters and strings

```csharp
char grade = 'A';
string course = "C# Basics";
```

`char` stores one UTF-16 character, while `string` stores a sequence of characters.

## Example 5 - String length and indexing

```csharp
string name = "Grace";
Console.WriteLine(name.Length);
Console.WriteLine(name[0]);
```

Strings expose useful members such as `Length`, and indexes start at zero.

## Example 6 - String immutability

```csharp
string word = "cat";
string louder = word.ToUpper();
Console.WriteLine(word);
Console.WriteLine(louder);
```

String methods return new strings; they do not mutate the original string.

## Example 7 - StringBuilder

```csharp
var builder = new System.Text.StringBuilder();
builder.Append("Hello");
builder.Append(", ");
builder.Append("world");
Console.WriteLine(builder.ToString());
```

`StringBuilder` is useful when you need to build text through many repeated edits.

## Example 8 - Value type copy

```csharp
int a = 10;
int b = a;
b = 20;
Console.WriteLine(a);
```

Assigning a value type copies the value, so changing `b` does not change `a`.

## Example 9 - Reference type copy

```csharp
var first = new List<string> { "red" };
var second = first;
second.Add("blue");
Console.WriteLine(first.Count);
```

Assigning a reference type copies the reference, so both variables can point at the same object.

## Example 10 - Value equality

```csharp
int x = 5;
int y = 5;
Console.WriteLine(x == y);
```

For numbers, `==` compares the actual values.

## Example 11 - Reference equality surprise

```csharp
var a = new object();
var b = new object();
Console.WriteLine(a == b);
```

Two separate objects are not equal by reference unless a type customizes equality.

## Example 12 - Nullable reference

```csharp
string? nickname = null;
Console.WriteLine(nickname?.Length);
```

The `?` marks that the variable may be null, and `?.` safely skips member access if it is null.

## Example 13 - Null coalescing

```csharp
string? displayName = null;
string nameToShow = displayName ?? "Guest";
```

`??` provides a fallback value when the left side is null.

## Example 14 - Null coalescing assignment

```csharp
List<string>? names = null;
names ??= new List<string>();
names.Add("Ada");
```

`??=` assigns a fallback only when the variable currently contains null.

## Example 15 - Implicit conversion

```csharp
int count = 10;
long bigger = count;
```

Widening conversions can happen automatically when they do not lose information.

## Example 16 - Explicit conversion

```csharp
double precise = 9.8;
int whole = (int)precise;
```

A cast is required here because converting from `double` to `int` can lose the fractional part.

## Example 17 - Safe parsing

```csharp
string input = "123";
if (int.TryParse(input, out int number))
{
    Console.WriteLine(number + 1);
}
```

`TryParse` avoids exceptions for normal invalid user input.

## Example 18 - Numeric formatting

```csharp
decimal total = 1234.5m;
Console.WriteLine(total.ToString("C"));
Console.WriteLine(total.ToString("N2"));
```

Format strings control how values appear without changing the underlying number.

## Example 19 - Object variable

```csharp
object value = "hello";
Console.WriteLine(value.GetType().Name);
```

`object` can hold any value, but you lose compile-time access to the specific type's members.

## Example 20 - Boxing

```csharp
int number = 42;
object boxed = number;
int unboxed = (int)boxed;
```

Boxing wraps a value type as an object, and unboxing requires a cast back to the original type.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `short small = 120;` | Stores a smaller integer range than `int`. |
| 22 | `uint positive = 42u;` | Stores a non-negative integer using an unsigned type. |
| 23 | `long id = 9_223_372_036L;` | Uses `long` for larger whole numbers. |
| 24 | `decimal tax = 7.25m;` | Uses decimal for base-10 financial values. |
| 25 | `double average = 98.6d;` | Uses double for common floating-point calculations. |
| 26 | `float opacity = 0.5f;` | Uses float when a smaller floating-point type is required. |
| 27 | `bool enabled = default;` | `default` for bool is `false`. |
| 28 | `int zero = default;` | `default` for int is `0`. |
| 29 | `string? missing = default;` | `default` for nullable reference types is null. |
| 30 | `var amount = 10m;` | The suffix makes the inferred type `decimal`. |
| 31 | `var ratio = 0.75;` | A floating-point literal without suffix usually infers `double`. |
| 32 | `string text = "hello";` | Strings are reference types with immutable contents. |
| 33 | `bool same = string.Equals(a, b, StringComparison.OrdinalIgnoreCase);` | Compares strings with an explicit comparison rule. |
| 34 | `string joined = string.Join(", ", names);` | Combines many strings with a separator. |
| 35 | `string repeated = new string('*', 10);` | Creates a string by repeating a character. |
| 36 | `bool starts = name.StartsWith("A");` | Checks whether a string begins with specific text. |
| 37 | `bool contains = email.Contains("@");` | Tests whether a substring appears inside a string. |
| 38 | `string part = text.Substring(0, 3);` | Extracts a portion of a string by index and length. |
| 39 | `string[] parts = csv.Split(',');` | Splits text into an array using a delimiter. |
| 40 | `int[] values = new int[3];` | Arrays are reference types even when their elements are value types. |
| 41 | `DateTime now = DateTime.UtcNow;` | Stores a date and time value. |
| 42 | `TimeSpan delay = TimeSpan.FromSeconds(5);` | Represents a duration of time. |
| 43 | `Guid id = Guid.NewGuid();` | Creates a globally unique identifier value. |
| 44 | `int? maybe = null;` | Makes a value type optional. |
| 45 | `int value = maybe.GetValueOrDefault();` | Reads a nullable value or returns the type default. |
| 46 | `int value = maybe ?? -1;` | Supplies a custom fallback for a nullable value. |
| 47 | `object boxed = 123;` | Boxes an integer so it can be stored as `object`. |
| 48 | `if (boxed is int n) { }` | Safely unboxes after a runtime type check. |
| 49 | `double widened = 10;` | Converts `int` to `double` implicitly. |
| 50 | `int narrowed = (int)10.9;` | Converts `double` to `int` explicitly and loses the fraction. |
| 51 | `decimal exact = Convert.ToDecimal("12.50");` | Uses `Convert` to transform compatible values. |
| 52 | `string formatted = amount.ToString("0.00");` | Formats a decimal with two digits after the decimal point. |
| 53 | `string hex = number.ToString("X");` | Formats an integer as hexadecimal text. |
| 54 | `string nameOf = nameof(Customer.Name);` | Gets a member name as a compile-time-safe string. |
| 55 | `var tuple = (Name: "Ada", Age: 36);` | Creates a small grouped value with named fields. |
| 56 | `dynamic late = "hello";` | Defers member checks until runtime, which is flexible but risky. |
| 57 | `nint nativeInt = 5;` | Stores an integer sized for the current platform. |
| 58 | `byte b = 0b_1010_0001;` | Uses a binary numeric literal. |
| 59 | `int mask = 0xFF;` | Uses a hexadecimal numeric literal. |
| 60 | `checked { int x = int.MaxValue + 1; }` | Requests overflow checking for integer arithmetic. |
