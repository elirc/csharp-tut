# 03 - Control Flow Examples

These examples reinforce comparisons, branching, switch logic, loops, `break`, `continue`, and early returns.

## Example 1 - Boolean comparison

```csharp
int age = 20;
bool canVote = age >= 18;
```

Comparison operators produce `bool` values that control decisions.

## Example 2 - Combining conditions

```csharp
bool hasTicket = true;
bool isBanned = false;
bool canEnter = hasTicket && !isBanned;
```

`&&`, `||`, and `!` let you build larger conditions from smaller ones.

## Example 3 - Basic if

```csharp
int score = 82;
if (score >= 70)
{
    Console.WriteLine("Passing");
}
```

An `if` block runs only when its condition is true.

## Example 4 - if else

```csharp
int temperature = 95;
if (temperature > 90)
{
    Console.WriteLine("Hot");
}
else
{
    Console.WriteLine("Comfortable");
}
```

`else` gives you a fallback path when the first condition fails.

## Example 5 - else if chain

```csharp
int score = 91;
if (score >= 90) Console.WriteLine("A");
else if (score >= 80) Console.WriteLine("B");
else Console.WriteLine("Keep practicing");
```

An `else if` chain checks conditions from top to bottom and stops at the first match.

## Example 6 - Ternary operator

```csharp
int quantity = 12;
string label = quantity == 1 ? "item" : "items";
```

The ternary operator is a compact expression for choosing between two values.

## Example 7 - Switch statement

```csharp
string role = "admin";
switch (role)
{
    case "admin":
        Console.WriteLine("Full access");
        break;
    default:
        Console.WriteLine("Limited access");
        break;
}
```

A switch statement branches based on matching cases.

## Example 8 - Switch expression

```csharp
string grade = "B";
int points = grade switch
{
    "A" => 4,
    "B" => 3,
    "C" => 2,
    _ => 0
};
```

A switch expression is ideal when you want to produce a value from several cases.

## Example 9 - Switch with ranges

```csharp
int score = 87;
string result = score switch
{
    >= 90 => "Excellent",
    >= 70 => "Passing",
    _ => "Needs work"
};
```

Relational patterns make range-based decisions readable.

## Example 10 - For loop

```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);
}
```

Use `for` when you need an index or know exactly how many times to loop.

## Example 11 - Counting backward

```csharp
for (int i = 5; i > 0; i--)
{
    Console.WriteLine(i);
}
```

The update expression can decrement just as easily as increment.

## Example 12 - Foreach loop

```csharp
string[] names = { "Ada", "Grace", "Linus" };
foreach (string name in names)
{
    Console.WriteLine(name);
}
```

`foreach` is the preferred way to read every item in a collection.

## Example 13 - While loop

```csharp
int attempts = 0;
while (attempts < 3)
{
    attempts++;
}
```

Use `while` when the loop should continue as long as a condition remains true.

## Example 14 - Do while loop

```csharp
string? input;
do
{
    input = Console.ReadLine();
}
while (input != "quit");
```

`do...while` always runs the body at least once before checking the condition.

## Example 15 - Break

```csharp
foreach (int number in new[] { 2, 4, 7, 8 })
{
    if (number % 2 != 0) break;
    Console.WriteLine(number);
}
```

`break` exits the nearest loop immediately.

## Example 16 - Continue

```csharp
for (int i = 1; i <= 5; i++)
{
    if (i == 3) continue;
    Console.WriteLine(i);
}
```

`continue` skips the rest of the current loop iteration.

## Example 17 - Early return

```csharp
static void PrintName(string? name)
{
    if (string.IsNullOrWhiteSpace(name)) return;
    Console.WriteLine(name);
}
```

An early return handles a simple exit case before the main work.

## Example 18 - Nested conditions

```csharp
bool loggedIn = true;
bool isAdmin = false;

if (loggedIn)
{
    Console.WriteLine(isAdmin ? "Admin area" : "User area");
}
```

Nested conditions model decisions that depend on earlier decisions.

## Example 19 - Looping until valid input

```csharp
int number;
while (!int.TryParse(Console.ReadLine(), out number))
{
    Console.WriteLine("Enter a number.");
}
```

This pattern repeats until parsing succeeds without throwing exceptions.

## Example 20 - Putting decisions and loops together

```csharp
int[] scores = { 95, 62, 80, 40 };
foreach (int score in scores)
{
    string status = score >= 70 ? "pass" : "retry";
    Console.WriteLine($"{score}: {status}");
}
```

Real programs often combine iteration with per-item decisions.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `bool adult = age >= 18;` | Stores the result of a comparison for later use. |
| 22 | `bool valid = age >= 0 && age <= 120;` | Combines two range checks with logical AND. |
| 23 | `bool outside = age < 0 \|\| age > 120;` | Uses logical OR when either condition should pass. |
| 24 | `if (!isReady) return;` | Negates a boolean to handle the not-ready case first. |
| 25 | `if (name is null) return;` | Uses pattern syntax for a null guard. |
| 26 | `if (name is not null) Console.WriteLine(name);` | Narrows a nullable variable after a positive check. |
| 27 | `string label = count == 1 ? "item" : "items";` | Chooses between two values with the ternary operator. |
| 28 | `string access = role switch { "Admin" => "All", _ => "Basic" };` | Maps a role to an access label with a switch expression. |
| 29 | `string band = score switch { >= 90 => "A", >= 80 => "B", _ => "C" };` | Uses ordered relational patterns for grading. |
| 30 | `switch (command) { case "q": return; }` | Uses a switch statement for command-style branching. |
| 31 | `for (int i = 0; i < items.Count; i++) { }` | Uses `for` when you need an index. |
| 32 | `for (int i = items.Count - 1; i >= 0; i--) { }` | Iterates backward through an indexed collection. |
| 33 | `foreach (var item in items) { }` | Uses `foreach` for simple collection traversal. |
| 34 | `while (queue.Count > 0) { queue.Dequeue(); }` | Repeats while a collection still has work. |
| 35 | `do { input = Read(); } while (input == "");` | Runs once before checking whether to repeat. |
| 36 | `if (number < 0) continue;` | Skips one loop iteration for unwanted values. |
| 37 | `if (found) break;` | Stops a loop after the goal is reached. |
| 38 | `return total;` | Ends a method and sends a value back to the caller. |
| 39 | `if (items.Count == 0) return 0;` | Handles an empty input before doing main work. |
| 40 | `bool inRange = value is >= 1 and <= 10;` | Uses pattern logic for a readable range check. |
| 41 | `bool isWeekend = day is DayOfWeek.Saturday or DayOfWeek.Sunday;` | Uses pattern OR for named alternatives. |
| 42 | `if (text is { Length: > 0 }) { }` | Checks that a string is non-null and non-empty. |
| 43 | `if (order is { IsPaid: true }) { }` | Branches based on an object property. |
| 44 | `var result = success ? value : fallback;` | Keeps simple two-way assignment compact. |
| 45 | `if (a > b) (a, b) = (b, a);` | Uses a conditional swap to enforce ordering. |
| 46 | `for (int row = 0; row < 3; row++) { for (int col = 0; col < 3; col++) { } }` | Shows nested loops for grid-like work. |
| 47 | `foreach (char ch in text) { }` | Iterates through characters in a string. |
| 48 | `while (true) { if (Done()) break; }` | Uses an intentional loop with an explicit exit condition. |
| 49 | `if (TryLoad(out var value)) Use(value);` | Branches only when a Try-style method succeeds. |
| 50 | `switch (ex) { case TimeoutException: break; }` | Switches on exception types when handling failures. |
| 51 | `string kind = obj switch { string => "text", int => "number", _ => "other" };` | Uses type patterns in a switch expression. |
| 52 | `if (list.Count is 0) return;` | Uses a constant pattern instead of equality. |
| 53 | `if (index < 0 \|\| index >= list.Count) return;` | Guards against invalid indexes. |
| 54 | `if (permissions.Contains("read")) { }` | Branches based on collection membership. |
| 55 | `int attempts = 0; while (++attempts <= 3) { }` | Combines incrementing and checking in one loop condition. |
| 56 | `for (int i = 0; i < 10; i += 2) { }` | Advances a loop by two each iteration. |
| 57 | `foreach (var pair in map) Console.WriteLine(pair.Key);` | Iterates key/value pairs from a dictionary. |
| 58 | `if (string.IsNullOrWhiteSpace(input)) return;` | Rejects empty user input early. |
| 59 | `var message = errors.Any() ? "Invalid" : "OK";` | Chooses a label from a collection condition. |
| 60 | `if (debug) Console.WriteLine(state);` | Uses a simple flag to include optional behavior. |
