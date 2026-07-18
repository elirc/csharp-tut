# 03 — Control Flow

How code branches and loops. This is mostly familiar from any language, but C# has some modern conveniences (switch expressions, pattern matching in `switch`) worth learning properly. Pattern matching gets its own deep dive in Chapter 14; here we cover the everyday forms.

## Booleans and comparison

```csharp
bool b = true;

// Comparison operators produce bool:
5 == 5;    // equal
5 != 3;    // not equal
5 > 3;     // greater
5 >= 5;    // greater or equal
3 < 5;     // less
3 <= 5;    // less or equal

// Logical operators:
a && b;    // AND (short-circuits: if a is false, b isn't evaluated)
a || b;    // OR  (short-circuits: if a is true, b isn't evaluated)
!a;        // NOT
```

Short-circuiting matters: `if (obj != null && obj.IsActive)` is safe because the right side only runs when `obj` isn't null.

## `if` / `else if` / `else`

```csharp
int score = 75;

if (score >= 90)
{
    Console.WriteLine("A");
}
else if (score >= 80)
{
    Console.WriteLine("B");
}
else if (score >= 70)
{
    Console.WriteLine("C");
}
else
{
    Console.WriteLine("F");
}
```

Braces are optional for single statements but **use them anyway** — bracket-less `if`s cause real bugs (the famous "goto fail" class of mistake). Team style guides almost always require braces.

### Ternary conditional operator

For a simple either/or value:

```csharp
string result = score >= 60 ? "Pass" : "Fail";
int max = a > b ? a : b;
```

Don't nest ternaries deeply — it gets unreadable fast. Two levels max.

## `switch` statement

Good when comparing one value against many constants:

```csharp
switch (dayNumber)
{
    case 1:
        Console.WriteLine("Monday");
        break;          // required — C# doesn't fall through (unlike C/Java)
    case 6:
    case 7:             // multiple labels share a body
        Console.WriteLine("Weekend");
        break;
    default:
        Console.WriteLine("Weekday");
        break;
}
```

Each `case` must end with `break` (or `return`/`throw`) — C# forbids accidental fall-through. Stacking labels (`case 6: case 7:`) is how you share a body.

## `switch` expression (modern, very common)

Since C# 8, `switch` can be an *expression* that returns a value. This is idiomatic modern C# — you'll see and write it constantly:

```csharp
string dayName = dayNumber switch
{
    1 => "Monday",
    2 => "Tuesday",
    6 or 7 => "Weekend",
    _ => "Other"          // _ is the catch-all (like default)
};
```

It's concise and the compiler warns if you don't handle all cases. It also supports **pattern matching**:

```csharp
string Describe(int n) => n switch
{
    < 0 => "negative",
    0 => "zero",
    > 0 and < 100 => "small",
    _ => "large"
};

string category = (age, hasLicense) switch    // switch on a tuple
{
    (< 16, _) => "too young",
    (>= 16, true) => "can drive",
    (>= 16, false) => "needs a license",
};
```

We go deeper on patterns in Chapter 14. For now, know switch expressions exist and prefer them when mapping an input to an output value.

## Loops

### `for` — when you know the count / need an index

```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);     // 0,1,2,3,4
}

// Count down:
for (int i = 10; i > 0; i--) { /* ... */ }

// Step by 2:
for (int i = 0; i < 20; i += 2) { /* ... */ }
```

The three parts are: initializer; condition (checked before each iteration); iterator (runs after each iteration).

### `foreach` — iterate a collection (preferred)

```csharp
string[] names = { "Ann", "Bob", "Cy" };
foreach (string name in names)
{
    Console.WriteLine(name);
}

// Works on anything enumerable: List, Dictionary, arrays, query results...
var ages = new Dictionary<string, int> { ["Ann"] = 30, ["Bob"] = 25 };
foreach (var pair in ages)
    Console.WriteLine($"{pair.Key} is {pair.Value}");

// Deconstruct the pair directly:
foreach (var (name, age) in ages)
    Console.WriteLine($"{name} is {age}");
```

Use `foreach` whenever you just need each element. It's clearer and avoids off-by-one errors. **Don't modify the collection (add/remove) while foreach-ing it** — that throws `InvalidOperationException`.

### `while` — loop while a condition holds

```csharp
int n = 1;
while (n < 100)
{
    n *= 2;
}
// n is now 128
```

### `do...while` — runs the body at least once

```csharp
string? input;
do
{
    Console.Write("Type 'quit': ");
    input = Console.ReadLine();
} while (input != "quit");
```

## `break` and `continue`

```csharp
foreach (var item in items)
{
    if (item.IsDeleted)
        continue;       // skip to the next iteration

    if (item.IsFatal)
        break;          // exit the loop entirely

    Process(item);
}
```

- `continue` — skip the rest of this iteration, move to the next.
- `break` — stop the loop immediately.

## `return`

Exits the current method, optionally with a value:

```csharp
bool IsEven(int n)
{
    return n % 2 == 0;
}
```

Returning early is good style — it flattens nested `if`s (guard clauses):

```csharp
decimal CalculateDiscount(Order order)
{
    if (order is null) return 0;          // guard
    if (order.Total < 100) return 0;      // guard
    if (!order.Customer.IsLoyal) return 5;
    return 15;
}
```

This reads top-to-bottom and avoids deep nesting. Prefer it over one giant nested `if/else`.

## `goto` (don't)

C# has `goto`. You will essentially never use it. Mentioned only so you recognize it as a smell if you see it.

## Putting it together

```csharp
// FizzBuzz — the classic
for (int i = 1; i <= 20; i++)
{
    string output = (i % 3, i % 5) switch
    {
        (0, 0) => "FizzBuzz",
        (0, _) => "Fizz",
        (_, 0) => "Buzz",
        _ => i.ToString()
    };
    Console.WriteLine(output);
}
```

## Exercises

1. Write a loop that prints the multiplication table for a number the user enters (1×N through 12×N).
2. Read numbers from the console until the user types `done`, then print the sum, count, and average. (Use `while` + `TryParse`.)
3. Rewrite a grade classifier (`A`/`B`/`C`/`D`/`F` from a 0–100 score) as a `switch` expression using relational patterns.
4. Write a guard-clause method `string Validate(string? email)` that returns an error message string for: null/empty, missing `@`, or otherwise returns `"OK"`.

---

**Next:** [04 — Methods](./04-methods.md)
