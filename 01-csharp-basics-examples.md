# 01 - C# Basics Examples

These snippets reinforce program structure, console I/O, strings, comments, variables, and constants.

## Example 1 - Top-level statements

```csharp
Console.WriteLine("Hello from C#!");
```

Modern console apps can start with statements directly, without writing a `Program` class by hand.

## Example 2 - Full program form

```csharp
class Program
{
    static void Main()
    {
        Console.WriteLine("Full form");
    }
}
```

This older shape is still useful to recognize because many examples and legacy projects use it.

## Example 3 - Using a namespace

```csharp
using System;

Console.WriteLine(DateTime.Now);
```

The `using` directive lets you refer to `DateTime` without writing `System.DateTime`.

## Example 4 - File-scoped namespace

```csharp
namespace TutorialApp;

public class Greeter
{
    public string SayHello() => "Hello";
}
```

A file-scoped namespace keeps the file less indented while still grouping types logically.

## Example 5 - Statements end with semicolons

```csharp
int score = 10;
score += 5;
Console.WriteLine(score);
```

Most C# statements end with `;`, while blocks use braces to group multiple statements.

## Example 6 - A simple block

```csharp
{
    int localNumber = 42;
    Console.WriteLine(localNumber);
}
```

Blocks create scope, so variables declared inside are not available outside the braces.

## Example 7 - Single-line comment

```csharp
// Calculate tax before displaying the total.
decimal tax = 19.99m * 0.08m;
```

Comments should explain intent when the code itself does not make the reason obvious.

## Example 8 - Multi-line comment

```csharp
/*
  This is useful for temporarily explaining
  a larger block of learning code.
*/
Console.WriteLine("Comments do not run.");
```

Multi-line comments can span several lines, but production code usually keeps them short.

## Example 9 - Console output

```csharp
Console.Write("Name: ");
Console.WriteLine("Ada");
```

`Write` stays on the same line, while `WriteLine` adds a newline afterward.

## Example 10 - Console input

```csharp
Console.Write("What is your name? ");
string? name = Console.ReadLine();
Console.WriteLine($"Hi, {name}!");
```

`ReadLine` returns nullable `string?` because the input stream can end without producing text.

## Example 11 - String interpolation

```csharp
string product = "book";
decimal price = 12.50m;
Console.WriteLine($"{product} costs {price:C}");
```

Interpolation embeds expressions inside a string, and `:C` formats the number as currency.

## Example 12 - Verbatim string

```csharp
string path = @"C:\Users\Owner\Desktop";
Console.WriteLine(path);
```

The `@` prefix keeps backslashes literal, which is useful for Windows paths.

## Example 13 - Raw string literal

```csharp
string json = """
{
  "name": "Ada",
  "language": "C#"
}
""";
```

Raw strings make multi-line text easier because you do not need to escape quotes inside it.

## Example 14 - Explicit types

```csharp
int count = 3;
double temperature = 72.5;
bool isReady = true;
```

Explicit type declarations are clear when you want the reader to see the exact type immediately.

## Example 15 - Type inference with var

```csharp
var count = 3;
var message = "Learning C#";
```

`var` asks the compiler to infer the type from the value on the right side.

## Example 16 - Constants

```csharp
const decimal SalesTaxRate = 0.0825m;
decimal tax = 100m * SalesTaxRate;
```

Constants are named values that cannot change after compilation.

## Example 17 - Naming matters

```csharp
int daysUntilDeadline = 5;
bool hasSubmittedAssignment = false;
```

Good variable names reduce the need for comments because the code explains its own role.

## Example 18 - Basic arithmetic

```csharp
int subtotal = 30;
int shipping = 7;
int total = subtotal + shipping;
```

Variables can be combined with operators to produce new values.

## Example 19 - Reading and parsing

```csharp
Console.Write("Age: ");
string? input = Console.ReadLine();
int age = int.Parse(input!);
```

`Parse` converts text to a number, and the `!` tells the compiler you believe `input` is not null.

## Example 20 - A small complete script

```csharp
Console.Write("First name: ");
string? firstName = Console.ReadLine();

Console.Write("Favorite number: ");
int favoriteNumber = int.Parse(Console.ReadLine()!);

Console.WriteLine($"{firstName}'s favorite number doubled is {favoriteNumber * 2}.");
```

This combines input, parsing, variables, arithmetic, and interpolation in one simple flow.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `Console.WriteLine("Welcome");` | Prints a line of output and moves to the next line. |
| 22 | `Console.Write("Name: ");` | Prints output without adding a newline. |
| 23 | `string? input = Console.ReadLine();` | Reads a line of user input that might be null. |
| 24 | `Console.Clear();` | Clears the console window before printing fresh output. |
| 25 | `Console.Title = "Tutorial";` | Sets the console window title. |
| 26 | `Console.ForegroundColor = ConsoleColor.Green;` | Changes text color for later console output. |
| 27 | `Console.ResetColor();` | Restores the console colors to their defaults. |
| 28 | `var today = DateTime.Today;` | Uses type inference for a clearly named value. |
| 29 | `const int MaxAttempts = 3;` | Gives a fixed value a meaningful name. |
| 30 | `string full = first + " " + last;` | Concatenates strings with the `+` operator. |
| 31 | `string full = $"{first} {last}";` | Uses interpolation to build a readable string. |
| 32 | `Console.WriteLine($"{price:C}");` | Formats a number as currency inside interpolation. |
| 33 | `Console.WriteLine($"{ratio:P1}");` | Formats a number as a percentage with one decimal place. |
| 34 | `Console.WriteLine($"{count,5}");` | Pads output to help align console columns. |
| 35 | `string path = @"C:\Temp\notes.txt";` | Uses a verbatim string so backslashes stay literal. |
| 36 | `string quote = "She said \"hi\"";` | Escapes quote characters inside a normal string. |
| 37 | `string multi = """line one\nline two""";` | Uses a raw string shape for multi-line text. |
| 38 | `// TODO: validate input` | Marks a small future task in a comment. |
| 39 | `int score = 0;` | Declares a variable with an explicit type and initial value. |
| 40 | `score += 10;` | Adds to the current value using a compound assignment. |
| 41 | `score++;` | Increments a numeric variable by one. |
| 42 | `score--;` | Decrements a numeric variable by one. |
| 43 | `bool isReady = true;` | Stores a true-or-false value. |
| 44 | `char initial = 'A';` | Stores a single character literal. |
| 45 | `decimal subtotal = 19.99m;` | Uses the `m` suffix for decimal literals. |
| 46 | `double distance = 12.5;` | Stores a floating-point measurement. |
| 47 | `int parsed = int.Parse("42");` | Converts valid numeric text to an integer. |
| 48 | `bool ok = int.TryParse(text, out int value);` | Attempts conversion without throwing on invalid input. |
| 49 | `string upper = name.ToUpperInvariant();` | Calls an instance method on a string. |
| 50 | `string trimmed = name.Trim();` | Removes whitespace from both ends of a string. |
| 51 | `bool blank = string.IsNullOrWhiteSpace(name);` | Checks for null, empty, or whitespace-only text. |
| 52 | `using System.Text;` | Imports a namespace so its types are easier to reference. |
| 53 | `namespace MyApp;` | Declares a file-scoped namespace. |
| 54 | `{ Console.WriteLine("block"); }` | Uses braces to create a block scope. |
| 55 | `int local = 1;` | Creates a local variable scoped to the current block. |
| 56 | `Console.WriteLine(args.Length);` | Reads command-line argument count in a top-level program. |
| 57 | `Environment.ExitCode = 1;` | Sets the process exit code for scripts or CI. |
| 58 | `Console.Error.WriteLine("Oops");` | Writes to standard error instead of standard output. |
| 59 | `Console.ReadKey();` | Waits for a key press in an interactive console app. |
| 60 | `return;` | Exits early from the current method or top-level program. |
