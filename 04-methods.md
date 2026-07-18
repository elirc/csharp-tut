# 04 — Methods

Methods are named, reusable blocks of behavior. C# has a rich set of parameter features. This chapter covers signatures, overloading, parameter modifiers, optional/named arguments, expression-bodied members, and local functions.

## Anatomy of a method

```csharp
public        int        Add      (int a, int b)
//  ^access    ^return     ^name     ^parameters
{
    return a + b;          // body
}
```

- **Access modifier** (`public`, `private`, etc.) — who can call it. Covered fully in Chapter 5. Default is `private`.
- **Return type** — `void` if it returns nothing.
- **Name** — `PascalCase` by convention for methods.
- **Parameters** — typed inputs.

```csharp
void Greet(string name)            // returns nothing
{
    Console.WriteLine($"Hi {name}");
}

int Square(int x)
{
    return x * x;
}
```

In top-level statement files you can declare methods as local functions (see below). Inside classes, they're members.

## Expression-bodied methods

When a method is a single expression, use `=>` (the "fat arrow"):

```csharp
int Square(int x) => x * x;
string Greet(string name) => $"Hi {name}";
bool IsEven(int n) => n % 2 == 0;
void Log(string msg) => Console.WriteLine(msg);   // works for void too
```

This is purely shorthand and very common for short methods, properties, and getters.

## Parameters and arguments

A **parameter** is the variable in the declaration; an **argument** is the actual value you pass.

### How arguments are passed: by value (default)

By default, C# passes arguments **by value** — the method gets a *copy* of the argument. For value types, that copies the data. For reference types, that copies the *reference* (so both point at the same object — you can mutate the object, but reassigning the parameter doesn't affect the caller).

```csharp
void TryToChange(int x)
{
    x = 999;            // changes only the local copy
}
int a = 5;
TryToChange(a);
Console.WriteLine(a);   // still 5

void Rename(Customer c)
{
    c.Name = "Changed"; // mutates the SHARED object — caller sees this
    c = new Customer(); // reassigning the local copy — caller does NOT see this
}
```

This trips people up. Re-read Chapter 2's value-vs-reference section if it's fuzzy. Mutating an object passed in *is* visible to the caller; replacing the parameter with a new object is *not*.

### `ref` — pass by reference

`ref` lets a method read **and** modify the caller's variable itself:

```csharp
void Increment(ref int x)
{
    x++;
}
int n = 5;
Increment(ref n);       // must write 'ref' at the call site too
Console.WriteLine(n);   // 6
```

You need `ref` on both the parameter and the argument. Used sparingly — usually for performance or swap-like operations.

### `out` — return extra values

`out` is like `ref` but the method **must** assign it; the caller doesn't need to initialize it. This is how `TryParse` returns both a success bool and the value:

```csharp
bool TryDivide(int a, int b, out int result)
{
    if (b == 0)
    {
        result = 0;
        return false;
    }
    result = a / b;
    return true;
}

if (TryDivide(10, 2, out int q))      // declare the variable inline
    Console.WriteLine(q);              // 5
```

The `Try...` pattern (return `bool`, value via `out`) is idiomatic across the BCL for "this might fail and that's normal."

### `in` — pass by reference, read-only

`in` passes by reference for performance (avoids copying a big struct) but forbids modification. You'll rarely write it; mostly relevant for large structs in performance-sensitive code.

### `params` — variable number of arguments

```csharp
int Sum(params int[] numbers)
{
    int total = 0;
    foreach (int n in numbers) total += n;
    return total;
}
Sum(1, 2, 3);          // 6
Sum(1, 2, 3, 4, 5);    // 15
Sum();                 // 0
int[] arr = {1, 2, 3};
Sum(arr);              // can also pass an array directly
```

`Console.WriteLine` and `string.Format` use `params` under the hood. Only one `params` parameter, and it must be last.

## Optional parameters and default values

```csharp
void Connect(string host, int port = 5432, bool useTls = true)
{
    // ...
}
Connect("db.local");                 // port=5432, useTls=true
Connect("db.local", 5433);           // useTls=true
Connect("db.local", 5433, false);
```

Parameters with defaults must come after required ones.

## Named arguments

You can pass arguments by name, in any order, which improves readability for booleans/optionals:

```csharp
Connect(host: "db.local", useTls: false);   // skip port, set useTls by name
CreateUser(name: "Sam", isAdmin: true);      // clearer than CreateUser("Sam", true)
```

Named arguments are great for avoiding "what does that `true` mean?" at call sites.

## Method overloading

Multiple methods can share a name if their **parameter lists differ** (by count or types). The compiler picks the right one:

```csharp
void Print(int x)    => Console.WriteLine($"int: {x}");
void Print(string s) => Console.WriteLine($"string: {s}");
void Print(double d) => Console.WriteLine($"double: {d}");

Print(5);        // int
Print("hi");     // string
Print(3.14);     // double
```

Return type alone does **not** distinguish overloads — the parameters must differ. Overloading is common (e.g., constructors, `Convert.ToInt32` has many overloads).

## Local functions

You can declare a function *inside* another method. It can use the outer method's variables (a closure). Great for helper logic that's only relevant locally:

```csharp
int Factorial(int n)
{
    if (n < 0) throw new ArgumentException("n must be >= 0");
    return Compute(n);

    // local function — only visible inside Factorial
    int Compute(int x) => x <= 1 ? 1 : x * Compute(x - 1);
}
```

Local functions keep helpers close to where they're used and out of the class's public surface.

## Recursion

A method calling itself. Fine for tree/graph traversal and divide-and-conquer; watch for stack overflow on deep recursion (C# doesn't optimize tail calls reliably).

```csharp
int Fib(int n) => n < 2 ? n : Fib(n - 1) + Fib(n - 2);
```

## Static vs instance methods (preview)

Methods belong either to the **type** (`static`) or to an **instance** of it. In top-level/console examples methods are effectively static. We cover instance methods properly in Chapter 5.

```csharp
// Static: called on the type
int max = Math.Max(3, 7);
double r = Math.Sqrt(16);

// Instance: called on an object
string s = "hello";
string upper = s.ToUpper();      // ToUpper is an instance method on the string object
```

## Method design tips (mid-level habits)

- **One job per method.** If a method does five things, split it. Aim for methods that fit on a screen.
- **Name by what it does**: `CalculateTax`, `IsEligible`, `LoadCustomer`. Verbs for actions, `Is/Has/Can` for booleans.
- **Validate inputs early** with guard clauses and throw `ArgumentException`/`ArgumentNullException` for bad arguments (Chapter 11).
- **Prefer returning values over mutating parameters.** Easier to reason about and test.
- **Avoid `out` parameters except for the `Try` pattern.** Returning a tuple or a small result object is usually clearer.

```csharp
// Returning multiple values via a tuple instead of out params:
(int min, int max) MinMax(int[] xs)
{
    int min = xs[0], max = xs[0];
    foreach (var x in xs)
    {
        if (x < min) min = x;
        if (x > max) max = x;
    }
    return (min, max);
}

var (lo, hi) = MinMax(new[] { 3, 1, 9, 4 });   // lo=1, hi=9
```

## Exercises

1. Write `int Max(params int[] values)` that returns the largest, throwing `ArgumentException` if empty.
2. Write a `TryGetInitials(string fullName, out string initials)` method following the Try pattern.
3. Overload a `Describe` method for `int`, `double`, and `string`, each printing something type-appropriate.
4. Write a method `(decimal subtotal, decimal tax, decimal total) Checkout(decimal[] prices, decimal taxRate)` returning all three as a tuple.

---

**Next:** [05 — Classes & Objects](./05-classes-and-objects.md)
