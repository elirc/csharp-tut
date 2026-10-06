# 02 — Types & Variables

C# is **statically typed**: every variable has a type known at compile time, and the compiler stops you from misusing it. Understanding the type system — especially **value vs reference types** and **nullability** — prevents the bugs that plague juniors. Read this chapter carefully; it's foundational.

## Built-in value types

| C# keyword | .NET type | Size | Notes |
|---|---|---|---|
| `int` | `Int32` | 32-bit | Default for whole numbers |
| `long` | `Int64` | 64-bit | Big whole numbers, suffix `L` |
| `short`, `byte` | `Int16`, `Byte` | 16/8-bit | Rare; `byte` is 0–255 |
| `uint`, `ulong` | unsigned | | Rarely needed |
| `float` | `Single` | 32-bit | Floating point, suffix `f` |
| `double` | `Double` | 64-bit | Default for decimals |
| `decimal` | `Decimal` | 128-bit | **Exact** — use for money, suffix `m` |
| `bool` | `Boolean` | | `true`/`false` |
| `char` | `Char` | 16-bit | One Unicode UTF-16 unit, single quotes |

The keyword and the .NET type are the same thing — `int` *is* `System.Int32`. Use the keywords.

### float/double vs decimal — money matters

`double`/`float` are binary floating point and **cannot represent many decimal fractions exactly**:

```csharp
Console.WriteLine(0.1 + 0.2);          // 0.30000000000000004  (double)
Console.WriteLine(0.1m + 0.2m);        // 0.3                  (decimal — exact)
```

**Rule: use `decimal` for money and anything where exact decimal arithmetic matters.** Use `double` for scientific/measurement values where tiny rounding is fine and performance matters.

### Number literals and suffixes

```csharp
int hex = 0xFF;          // 255 in hex
int binary = 0b1010;     // 10 in binary
int million = 1_000_000; // underscores for readability
double d = 1.5e3;        // 1500.0 (scientific)
long l = 100L;
float f = 1.5f;
decimal m = 9.99m;
```

## Strings

`string` is a sequence of characters. It's technically a *reference* type but behaves like a value in practice and is **immutable** — any "modification" creates a new string.

```csharp
string s = "hello";
string upper = s.ToUpper();        // "HELLO" — s is unchanged
Console.WriteLine(s);              // still "hello"

// Common operations:
s.Length;                          // 5
s.Contains("ell");                 // true
s.StartsWith("he");                // true
s.IndexOf("l");                    // 2
s.Substring(1, 3);                 // "ell" (start index, length)
s.Replace("l", "L");               // "heLLo"
s.Split(',');                      // string[] split on comma
"  trim me  ".Trim();              // "trim me"
string.IsNullOrEmpty(s);           // false
string.IsNullOrWhiteSpace("  ");   // true
string.Join(", ", new[] {"a","b"});// "a, b"
$"{s} world";                      // interpolation
s[0];                              // 'h' (indexer returns a char)
```

### Building strings efficiently

Because strings are immutable, concatenating in a loop creates many throwaway strings. Use `StringBuilder` for heavy concatenation:

```csharp
using System.Text;

var sb = new StringBuilder();
for (int i = 0; i < 1000; i++)
    sb.Append(i).Append(',');
string result = sb.ToString();
```

For a handful of concatenations, `+` or interpolation is perfectly fine.

## Value types vs reference types — THE key concept

This distinction governs how assignment, copying, and equality work. Get it wrong and you'll have baffling bugs.

### Value types

`int`, `double`, `bool`, `char`, `decimal`, enums, and `struct`s are **value types**. A variable *holds the data itself*. Assigning **copies the value**.

```csharp
int a = 5;
int b = a;     // b gets a COPY of 5
b = 99;
Console.WriteLine(a);   // 5  — a is unaffected
Console.WriteLine(b);   // 99
```

### Reference types

`class`es (including `string`, arrays, `List<T>`, and your own classes) are **reference types**. A variable holds a *reference* (a pointer) to an object on the heap. Assigning **copies the reference**, so both variables point at the **same object**.

```csharp
class Box { public int Value; }

var x = new Box { Value = 5 };
var y = x;          // y points to the SAME Box as x
y.Value = 99;
Console.WriteLine(x.Value);   // 99  — they're the same object!
```

This is the #1 source of "why did my data change?" bugs. If you pass an object to a method and the method mutates it, the caller sees the change. (Passing the variable copies the *reference*, not the object — see Chapter 4.)

```
Value type:                 Reference type:
 a -> [ 5 ]                  x ----\
 b -> [ 5 ] (copy)                  >--> [ Box: Value=99 ]  (on the heap)
                             y ----/
```

### Equality differs too

```csharp
int a = 5, b = 5;
Console.WriteLine(a == b);              // True — compares values

var p1 = new Box { Value = 5 };
var p2 = new Box { Value = 5 };
Console.WriteLine(p1 == p2);            // False — different objects (reference equality)
Console.WriteLine(p1 == p1);            // True — same reference

// string is special: == compares CONTENTS, not references
string s1 = "hi", s2 = "h" + "i";
Console.WriteLine(s1 == s2);            // True
```

`record` types (Chapter 7) give you value-based equality for reference types, which is great for data models.

## `null` and nullability

`null` means "no object / no value." Reference types can be null. Value types cannot be null **unless** you make them nullable with `?`.

```csharp
string? maybeName = null;      // nullable string (reference type)
int? maybeAge = null;          // nullable int (value type) — Nullable<int>
int definitelyAge = 0;         // cannot be null

maybeAge = 25;
if (maybeAge.HasValue)
    Console.WriteLine(maybeAge.Value);   // 25
int age = maybeAge ?? 0;                 // ?? = "if null, use this default"
```

The dreaded **`NullReferenceException`** happens when you call a member on a null reference:

```csharp
string? name = null;
Console.WriteLine(name.Length);   // 💥 NullReferenceException at runtime
```

C# has **Nullable Reference Types** (the `Nullable` project setting) to catch these at compile time. We dedicate Chapter 15 to it — it's that important. For now, know that `string?` means "might be null" and `string` means "shouldn't be null," and the compiler warns you when you ignore the difference.

### Null-handling operators (you'll use these constantly)

```csharp
string? input = GetInput();

// ?? null-coalescing: provide a fallback
string name = input ?? "Anonymous";

// ??= assign only if currently null
input ??= "default";

// ?. null-conditional: safely access members; whole expression is null if left is null
int? len = input?.Length;          // null if input is null, else its length

// chain them:
string city = customer?.Address?.City ?? "Unknown";
```

These short-circuit on null, letting you avoid pyramids of `if (x != null)`.

## Type conversion

### Implicit (widening) — automatic and safe

```csharp
int i = 100;
long l = i;       // int fits in long, no data loss — automatic
double d = i;     // int -> double — automatic
```

### Explicit (narrowing) — requires a cast, may lose data

```csharp
double d = 9.99;
int i = (int)d;        // 9 — truncates the fraction (does NOT round)
long big = 5_000_000_000L;
int overflow = (int)big;   // 705032704 — keeps only the low 32 bits; no error (unchecked by default)
```

A cast says "I know this might lose data; do it anyway."

### Parsing strings to numbers

User input and file/network data arrive as strings. Convert carefully:

```csharp
// Parse: throws FormatException if invalid — only use when you're sure it's valid
int n = int.Parse("42");
decimal price = decimal.Parse("19.99");

// TryParse: the SAFE, idiomatic way. Returns bool; result via out parameter.
if (int.TryParse(userInput, out int value))
    Console.WriteLine($"Got {value}");
else
    Console.WriteLine("Not a valid number");
```

**Prefer `TryParse`** for any input you don't fully control. It avoids exceptions for the common "bad input" case.

`Convert.ToInt32(...)` exists too and handles null differently (returns 0), but `TryParse`/`Parse` are the standard tools.

### ToString and formatting

Every type has `.ToString()`. Numbers and dates accept format strings:

```csharp
int n = 1234567;
n.ToString();          // "1234567"
n.ToString("N0");      // "1,234,567"
(0.85).ToString("P0"); // "85%" in en-US — format output depends on the current culture
DateTime.Now.ToString("yyyy-MM-dd");  // "2026-06-12"
```

## `object` — the root type

Every type in C# ultimately derives from `object` (`System.Object`). A variable of type `object` can hold anything:

```csharp
object anything = 42;
anything = "now a string";
anything = new List<int>();
```

You rarely use `object` directly in modern code (generics replaced most uses), but you should know it exists and that `.ToString()`, `.Equals()`, and `.GetHashCode()` come from it.

### Boxing (know the term)

Putting a value type into an `object` (or interface) reference **boxes** it — wraps it in a heap object. Taking it back out **unboxes**. This costs performance, so avoid it in hot loops. Generics exist largely to avoid boxing.

```csharp
int x = 5;
object boxed = x;        // boxing: heap allocation
int y = (int)boxed;      // unboxing
```

## `nameof` and a couple of handy operators

```csharp
string n = nameof(Console);     // "Console" — compile-time name as string;
                                // refactor-safe, great for logging/validation messages

// typeof gets type metadata
Type t = typeof(string);

// is / as for type checks (more in Chapter 14 on pattern matching)
object o = "hello";
if (o is string str)            // checks type AND assigns to str if it matches
    Console.WriteLine(str.Length);
```

## Exercises

1. Demonstrate the value-vs-reference difference: write a `class Counter { public int Count; }`, copy it into another variable, increment one, and print both. Then do the same with a plain `int`. Explain the difference in a comment.
2. Show that `0.1 + 0.2 != 0.3` with `double` but is exact with `decimal`.
3. Read two numbers from the console using `TryParse`, and print their sum — but print a friendly error if either input is invalid.
4. Given `string? middleName = null;`, print `"N/A"` when it's null using `??`, and print its length safely using `?.`.

---

**Next:** [03 — Control Flow](./03-control-flow.md)
