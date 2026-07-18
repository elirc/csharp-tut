# 12 - Delegates, Events & Lambdas Examples

These snippets reinforce lambdas, `Func`, `Action`, `Predicate`, method groups, closures, multicast delegates, and events.

## Example 1 - Lambda expression

```csharp
Func<int, int> doubleIt = x => x * 2;
```

A lambda is an inline function you can store or pass around.

## Example 2 - Lambda with block body

```csharp
Func<int, int> abs = x =>
{
    return x < 0 ? -x : x;
};
```

Use a block body when the lambda needs multiple statements.

## Example 3 - Action delegate

```csharp
Action<string> print = message => Console.WriteLine(message);
```

`Action<T>` represents work that takes input and returns nothing.

## Example 4 - Func delegate

```csharp
Func<int, int, int> add = (a, b) => a + b;
```

`Func` returns a value, with the final generic argument representing the return type.

## Example 5 - Predicate delegate

```csharp
Predicate<string> isLong = text => text.Length > 10;
```

`Predicate<T>` is a conventional delegate for a boolean test.

## Example 6 - Passing behavior

```csharp
static IEnumerable<T> Filter<T>(IEnumerable<T> values, Predicate<T> predicate)
{
    foreach (T value in values)
        if (predicate(value)) yield return value;
}
```

Passing behavior lets one method reuse the same loop with different decisions.

## Example 7 - Method group

```csharp
Action<string> printer = Console.WriteLine;
```

A method group can be converted to a compatible delegate without writing a lambda.

## Example 8 - Custom delegate

```csharp
public delegate decimal Discount(decimal subtotal);
```

Custom delegates give a meaningful name to a function shape.

## Example 9 - Using a custom delegate

```csharp
Discount tenPercent = subtotal => subtotal * 0.10m;
decimal saved = tenPercent(100m);
```

Delegates are invoked like methods once assigned.

## Example 10 - Closure

```csharp
int minimum = 10;
Func<int, bool> isBigEnough = value => value >= minimum;
```

The lambda captures `minimum` from the surrounding scope.

## Example 11 - Closure sees changes

```csharp
int threshold = 5;
Func<int, bool> test = value => value > threshold;
threshold = 10;
Console.WriteLine(test(7));
```

Captured variables are captured by variable, so later changes are visible.

## Example 12 - Multicast delegate

```csharp
Action notify = () => Console.WriteLine("First");
notify += () => Console.WriteLine("Second");
notify();
```

Multicast delegates call each subscribed method in order.

## Example 13 - Removing a delegate

```csharp
void Log() => Console.WriteLine("Log");
Action action = Log;
action -= Log;
```

Delegates can be unsubscribed when you have the same method reference.

## Example 14 - Event declaration

```csharp
public class Button
{
    public event EventHandler? Clicked;
}
```

Events expose a controlled subscription model around delegates.

## Example 15 - Raising an event

```csharp
public class Button
{
    public event EventHandler? Clicked;
    public void Click() => Clicked?.Invoke(this, EventArgs.Empty);
}
```

The null-conditional operator safely raises the event only when subscribers exist.

## Example 16 - Subscribing to an event

```csharp
var button = new Button();
button.Clicked += (sender, args) => Console.WriteLine("Clicked");
```

Subscribers attach handlers with `+=`.

## Example 17 - Custom event args

```csharp
public class OrderPlacedEventArgs : EventArgs
{
    public required int OrderId { get; init; }
}
```

Custom event args carry event-specific data to subscribers.

## Example 18 - Event with custom args

```csharp
public event EventHandler<OrderPlacedEventArgs>? OrderPlaced;
```

`EventHandler<T>` is the standard shape for events that include data.

## Example 19 - LINQ lambda

```csharp
var shortNames = names.Where(name => name.Length < 5);
```

LINQ relies heavily on lambdas to express filtering and projection.

## Example 20 - Choosing delegate types

```csharp
Func<string, bool> isValid = text => !string.IsNullOrWhiteSpace(text);
Action<string> report = Console.WriteLine;
```

Use `Func` for returned values, `Action` for side effects, and events for publish/subscribe workflows.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `Func<int, bool> even = n => n % 2 == 0;` | Stores a boolean test as a value. |
| 22 | `Func<string, string> trim = s => s.Trim();` | Represents a transformation from one string to another. |
| 23 | `Func<int, int, int> multiply = (a, b) => a * b;` | A `Func` can accept multiple input parameters. |
| 24 | `Action save = () => repository.Save();` | Stores parameterless work to run later. |
| 25 | `Action<string, int> log = (m, c) => Console.WriteLine(m);` | `Action` supports multiple input parameters. |
| 26 | `Predicate<Customer> active = c => c.IsActive;` | Names a domain-specific boolean test. |
| 27 | `var adults = people.Where(p => p.Age >= 18);` | LINQ commonly receives lambdas as behavior. |
| 28 | `button.Clicked += OnClicked;` | Subscribes a named method to an event. |
| 29 | `button.Clicked -= OnClicked;` | Unsubscribes a named method from an event. |
| 30 | `button.Clicked += (_, _) => Console.WriteLine("clicked");` | Uses a lambda as a quick event handler. |
| 31 | `EventHandler handler = OnClicked;` | Stores an event-compatible method in a delegate variable. |
| 32 | `handler.Invoke(this, EventArgs.Empty);` | Invokes a delegate explicitly. |
| 33 | `handler?.Invoke(this, EventArgs.Empty);` | Invokes only when at least one subscriber exists. |
| 34 | `Comparison<string> compare = string.CompareOrdinal;` | Uses a built-in delegate shape for sorting comparisons. |
| 35 | `Converter<string, int> parse = int.Parse;` | Uses a built-in delegate for conversion. |
| 36 | `list.ForEach(Console.WriteLine);` | Passes a method group where an `Action<T>` is expected. |
| 37 | `numbers.Sort((a, b) => a.CompareTo(b));` | Supplies comparison behavior inline. |
| 38 | `int factor = 10; Func<int, int> scale = n => n * factor;` | Demonstrates a closure over a local variable. |
| 39 | `factor = 20;` | The closure sees the updated variable value. |
| 40 | `Func<int> next = () => ++count;` | A closure can preserve state between delegate calls. |
| 41 | `Action pipeline = StepOne; pipeline += StepTwo;` | Combines multiple actions into one multicast delegate. |
| 42 | `pipeline -= StepOne;` | Removes one method from a multicast delegate. |
| 43 | `Delegate[] handlers = pipeline.GetInvocationList();` | Inspects the methods currently in a multicast delegate. |
| 44 | `public event Action? Changed;` | Events can use simple delegate types when no sender data is needed. |
| 45 | `Changed?.Invoke();` | Raises a simple event safely. |
| 46 | `public event EventHandler<SavedEventArgs>? Saved;` | Generic event handlers carry strongly typed event data. |
| 47 | `Saved?.Invoke(this, new SavedEventArgs(id));` | Raises an event with custom data. |
| 48 | `Func<Task> work = async () => await SaveAsync();` | Async lambdas can be assigned to task-returning delegates. |
| 49 | `Func<int, Task<string>> load = async id => await LoadAsync(id);` | Async delegates can accept inputs and return results. |
| 50 | `Task.Run(() => ExpensiveWork());` | Passes a lambda to run CPU-bound work on the thread pool. |
| 51 | `items.Select(static x => x.Id)` | A static lambda avoids capturing local variables. |
| 52 | `Func<int, int> square = static x => x * x;` | Static lambdas are useful for pure calculations. |
| 53 | `IEnumerable<T> Filter<T>(IEnumerable<T> source, Func<T, bool> test)` | A delegate parameter makes a method reusable. |
| 54 | `void Configure(Action<Options> configure)` | Lets callers customize an object through a callback. |
| 55 | `configure(new Options());` | Invokes caller-provided behavior. |
| 56 | `Func<string, bool> hasText = s => !string.IsNullOrWhiteSpace(s);` | Wraps a common validation rule as a delegate. |
| 57 | `Func<Customer, string> keySelector = c => c.Email;` | Key selector delegates are common in sorting and grouping. |
| 58 | `Action<Exception> onError = ex => logger.LogError(ex, "Failed");` | Error callbacks can be represented as actions. |
| 59 | `event EventHandler? Completed;` | The standard event shape works for many notifications. |
| 60 | `delegate void ProgressChanged(int percent);` | Custom delegates are useful when a named callback shape improves readability. |
