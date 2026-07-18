# 12 — Delegates, Events & Lambdas

This chapter is about treating **functions as values** — passing behavior around, not just data. You've already used this (every LINQ lambda). Now we make it explicit. The practical payoff: callbacks, LINQ, event handling, and flexible APIs.

## Lambdas — inline anonymous functions

A **lambda** is a function written inline, with no name. Syntax: `parameters => body`.

```csharp
Func<int, int> square = x => x * x;             // one param, expression body
Func<int, int, int> add = (a, b) => a + b;      // two params
Action greet = () => Console.WriteLine("Hi");   // no params, no return
Func<int, bool> isEven = n => n % 2 == 0;

// Statement body (multiple lines) needs braces and an explicit return:
Func<int, string> describe = n =>
{
    if (n < 0) return "negative";
    return n == 0 ? "zero" : "positive";
};

square(5);     // 25
add(2, 3);     // 5
greet();
```

- Single expression: `x => x * x` (the result is returned implicitly).
- Block body: `x => { ...; return ...; }`.
- Parameter types are usually inferred; add them when needed: `(int x) => x * x`.

You already use lambdas all over LINQ: `Where(p => p.Age > 18)`. That lambda is a function passed as an argument.

## `Func`, `Action`, `Predicate` — the built-in delegate types

A **delegate** is a type that represents a function signature — a "variable that holds a method." You almost never declare custom delegate types anymore; the generic built-ins cover everything:

- **`Func<..., TResult>`** — a function that **returns** a value. The last type parameter is the return type.
  - `Func<int>` — takes nothing, returns int.
  - `Func<int, int>` — takes int, returns int.
  - `Func<string, int, bool>` — takes string and int, returns bool.
- **`Action<...>`** — a function that returns **void**.
  - `Action` — takes nothing.
  - `Action<string>` — takes a string, returns nothing.
- **`Predicate<T>`** — takes a `T`, returns `bool` (same as `Func<T, bool>`; used by some BCL methods).

```csharp
Func<double, double> sqrt = Math.Sqrt;          // a delegate can point to a named method
Action<string> log = Console.WriteLine;
Predicate<int> isPositive = n => n > 0;
```

### Passing behavior into methods

This is the real power — a method can accept a function and call it, letting callers customize behavior:

```csharp
// Generic "retry" helper that takes the work to do as a Func:
public static T Retry<T>(Func<T> operation, int maxAttempts = 3)
{
    for (int attempt = 1; ; attempt++)
    {
        try { return operation(); }
        catch when (attempt < maxAttempts)
        {
            Console.WriteLine($"Attempt {attempt} failed, retrying...");
        }
    }
}

var data = Retry(() => FetchFromApi());   // pass the operation as a lambda

// A method that takes an Action callback:
public void ForEachLine(string path, Action<string> handle)
{
    foreach (var line in File.ReadLines(path))
        handle(line);
}
ForEachLine("data.txt", line => Console.WriteLine(line.ToUpper()));
```

This pattern — "pass me a function describing the variable part" — shows up in LINQ, sorting, configuration callbacks, middleware, and dependency-injected strategies.

```csharp
// Custom sort via a comparison function:
people.Sort((a, b) => a.Age.CompareTo(b.Age));

// Filtering with an injected predicate:
List<T> Filter<T>(IEnumerable<T> items, Func<T, bool> match)
    => items.Where(match).ToList();
```

## Method group conversion

When a method matches a delegate's signature, you can pass the method *by name* (no lambda):

```csharp
names.Select(n => n.ToUpper());      // lambda
names.Select(s => Transform(s));     // lambda calling a method
// equivalent method group form when signatures line up:
list.ForEach(Console.WriteLine);     // pass Console.WriteLine directly
numbers.Where(IsEven);               // pass the IsEven method directly

bool IsEven(int n) => n % 2 == 0;
```

## Closures — lambdas capture variables

A lambda can use variables from the enclosing scope. It **captures** them (by reference), so it sees their current value when invoked:

```csharp
int threshold = 18;
Func<Person, bool> isAdult = p => p.Age >= threshold;   // captures 'threshold'
var adults = people.Where(isAdult).ToList();

// Building a configurable function:
Func<int, int> MakeMultiplier(int factor) => x => x * factor;  // captures 'factor'
var triple = MakeMultiplier(3);
triple(10);    // 30
```

**Closure gotcha — capturing a loop variable.** In modern C# the `foreach` variable is fresh per iteration (safe), but be careful when deferring execution:

```csharp
var actions = new List<Action>();
foreach (var i in Enumerable.Range(0, 3))
    actions.Add(() => Console.WriteLine(i));   // each captures its own i -> 0,1,2 (modern C#)
actions.ForEach(a => a());
```

Just be aware that closures capture *variables*, not snapshots — if a captured variable changes later, the lambda sees the new value.

## Multicast delegates

A delegate can hold **multiple** methods; invoking it calls them all in order. This underpins events:

```csharp
Action pipeline = () => Console.WriteLine("one");
pipeline += () => Console.WriteLine("two");     // add another
pipeline += () => Console.WriteLine("three");
pipeline();      // prints one, two, three
pipeline -= /* some handler */ null;            // can remove too
```

## Events — the publish/subscribe pattern

An **event** is a delegate field with restricted access: outsiders can subscribe (`+=`) and unsubscribe (`-=`) but can't invoke or overwrite it. It's how an object notifies others that something happened, without knowing who's listening.

```csharp
public class Stock
{
    // Declare an event using the standard EventHandler<T> delegate:
    public event EventHandler<PriceChangedEventArgs>? PriceChanged;

    private decimal _price;
    public decimal Price
    {
        get => _price;
        set
        {
            if (_price == value) return;
            var old = _price;
            _price = value;
            // Raise the event (the ?. guards against no subscribers):
            PriceChanged?.Invoke(this, new PriceChangedEventArgs(old, value));
        }
    }
}

public class PriceChangedEventArgs(decimal oldPrice, decimal newPrice) : EventArgs
{
    public decimal OldPrice { get; } = oldPrice;
    public decimal NewPrice { get; } = newPrice;
}

// Subscriber:
var stock = new Stock();
stock.PriceChanged += (sender, e) =>
    Console.WriteLine($"Price moved {e.OldPrice:C} -> {e.NewPrice:C}");

stock.Price = 100;   // triggers the handler
```

Conventions:
- Use `EventHandler<TEventArgs>` (or `EventHandler` for no data). The handler signature is `(object? sender, TEventArgs e)`.
- Raise with `MyEvent?.Invoke(this, args)` — the `?.` skips it when nobody's subscribed.
- Subscribe with `+=`, unsubscribe with `-=`. **Unsubscribe when done** — lingering subscriptions are a classic memory-leak source (the publisher keeps subscribers alive).

In typical CRUD/web work you'll *use* events more than design them (UI frameworks, some libraries), but you should understand the pattern. In ASP.NET backends, asynchronous messaging and DI-based notifications often replace in-process events.

## When to use what

- **Lambda** — inline behavior, especially for LINQ and callbacks. Daily use.
- **`Func`/`Action`** — parameter/field types for "a function goes here." Daily use.
- **Events** — an object broadcasting "something happened" to subscribers. Occasional.
- **Custom `delegate` type declarations** — rarely needed; `Func`/`Action` suffice.

## Exercises

1. Write `int Apply(int x, Func<int,int> f)` and call it with several lambdas (`x => x+1`, `x => x*x`).
2. Write a `Measure(Action work)` helper that times how long `work` takes (use `System.Diagnostics.Stopwatch`) and prints the elapsed ms.
3. Implement a tiny event system: a `Timer`-like class with an `event EventHandler Tick`; raise it in a loop and have a subscriber count the ticks.
4. Write `Func<int,int> Compose(Func<int,int> f, Func<int,int> g)` returning `x => f(g(x))`. Test it.

---

**Next:** [13 — Async & Await](./13-async-await.md)
