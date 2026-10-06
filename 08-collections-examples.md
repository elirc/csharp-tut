# 08 - Collections Examples

These snippets reinforce arrays, lists, dictionaries, sets, queues, stacks, interfaces, iterators, and thread-safety basics.

## Example 1 - Array creation

```csharp
int[] scores = { 90, 82, 77 };
```

Arrays have a fixed length and are efficient when the size is known.

## Example 2 - Array indexing

```csharp
string[] names = { "Ada", "Grace", "Linus" };
Console.WriteLine(names[0]);
```

Collection indexes start at zero, so `[0]` reads the first item.

## Example 3 - Array length

```csharp
int[] numbers = { 1, 2, 3 };
Console.WriteLine(numbers.Length);
```

Arrays use `Length`, while many other collections use `Count`.

## Example 4 - Index from end

```csharp
int[] values = { 10, 20, 30 };
Console.WriteLine(values[^1]);
```

`^1` means the last item in the collection.

## Example 5 - Range slice

```csharp
int[] values = { 10, 20, 30, 40 };
int[] middle = values[1..3];
```

Ranges select a portion of an array using a start index and an exclusive end index.

## Example 6 - List creation

```csharp
var names = new List<string> { "Ada", "Grace" };
names.Add("Linus");
```

`List<T>` is the default resizable collection for ordered items.

## Example 7 - Removing from a list

```csharp
var numbers = new List<int> { 1, 2, 3 };
numbers.Remove(2);
```

`Remove` deletes the first matching value if it exists.

## Example 8 - Insert into a list

```csharp
var tasks = new List<string> { "build", "test" };
tasks.Insert(1, "format");
```

Insertions in the middle of a list shift later items to the right.

## Example 9 - Dictionary creation

```csharp
var ages = new Dictionary<string, int>
{
    ["Ada"] = 36,
    ["Grace"] = 85
};
```

Dictionaries store key/value pairs for fast lookup by key.

## Example 10 - Dictionary lookup

```csharp
if (ages.TryGetValue("Ada", out int age))
{
    Console.WriteLine(age);
}
```

`TryGetValue` safely handles missing keys without throwing.

## Example 11 - Updating dictionary values

```csharp
var counts = new Dictionary<string, int>();
counts["error"] = counts.GetValueOrDefault("error") + 1;
```

This pattern reads a fallback value, increments it, and stores the new count.

## Example 12 - HashSet uniqueness

```csharp
var tags = new HashSet<string>();
tags.Add("csharp");
tags.Add("csharp");
Console.WriteLine(tags.Count);
```

`HashSet<T>` stores unique values and ignores duplicates.

## Example 13 - HashSet membership

```csharp
bool hasTag = tags.Contains("csharp");
```

Set membership checks are fast and expressive.

## Example 14 - Queue

```csharp
var queue = new Queue<string>();
queue.Enqueue("first");
queue.Enqueue("second");
Console.WriteLine(queue.Dequeue());
```

A queue is first-in, first-out.

## Example 15 - Stack

```csharp
var stack = new Stack<string>();
stack.Push("first");
stack.Push("second");
Console.WriteLine(stack.Pop());
```

A stack is last-in, first-out.

## Example 16 - IEnumerable parameter

```csharp
static void PrintAll(IEnumerable<string> values)
{
    foreach (string value in values) Console.WriteLine(value);
}
```

Accepting `IEnumerable<T>` lets callers pass arrays, lists, sets, and many other sequences.

## Example 17 - IReadOnlyList parameter

```csharp
static string First(IReadOnlyList<string> values)
{
    return values[0];
}
```

`IReadOnlyList<T>` communicates that the method needs indexing but will not mutate the list.

## Example 18 - Iterator method

```csharp
static IEnumerable<int> CountTo(int max)
{
    for (int i = 1; i <= max; i++)
    {
        yield return i;
    }
}
```

`yield return` produces sequence items one at a time.

## Example 19 - Collection initializer

```csharp
var numbers = new List<int> { 1, 2, 3, 4 };
```

Collection initializers keep simple setup compact and readable.

## Example 20 - Concurrent collection

```csharp
var bag = new System.Collections.Concurrent.ConcurrentBag<int>();
bag.Add(1);
```

Concurrent collections are designed for multi-threaded access where normal collections are unsafe.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `Array.Sort(scores);` | Sorts an array in place. |
| 22 | `Array.Reverse(scores);` | Reverses the order of array elements in place. |
| 23 | `int index = Array.IndexOf(scores, 90);` | Finds the first index of a value in an array. |
| 24 | `var copy = scores.ToArray();` | Creates a new array copy from a sequence. |
| 25 | `List<int> numbers = new();` | Creates an empty resizable list. |
| 26 | `numbers.AddRange(new[] { 1, 2, 3 });` | Appends several items to a list. |
| 27 | `numbers.RemoveAt(0);` | Removes a list item by index. |
| 28 | `numbers.Clear();` | Removes every item from the list. |
| 29 | `numbers.Contains(5);` | Checks whether a list contains a value. |
| 30 | `numbers.Find(n => n > 10);` | Finds the first item matching a predicate. |
| 31 | `numbers.Exists(n => n < 0);` | Tests whether any list item matches. |
| 32 | `numbers.Sort();` | Sorts a list in place. |
| 33 | `var map = new Dictionary<string, int>();` | Creates an empty key/value lookup. |
| 34 | `map.Add("a", 1);` | Adds a new dictionary key and value. |
| 35 | `map["a"] = 2;` | Adds or replaces a dictionary value by key. |
| 36 | `map.Remove("a");` | Removes a key/value pair by key. |
| 37 | `map.ContainsKey("a");` | Checks whether a dictionary has a key. |
| 38 | `foreach (var key in map.Keys) { }` | Iterates through dictionary keys. |
| 39 | `foreach (var value in map.Values) { }` | Iterates through dictionary values. |
| 40 | `foreach (var item in map) { Console.WriteLine(item.Key); }` | Iterates through dictionary entries. |
| 41 | `var set = new HashSet<int>();` | Creates a collection that stores unique values. |
| 42 | `set.Add(1);` | Adds a value if it is not already present. |
| 43 | `set.Remove(1);` | Removes a value from a set. |
| 44 | `set.SetEquals(other);` | Checks whether two sets contain the same values. |
| 45 | `set.IntersectWith(other);` | Mutates a set to keep only shared values. |
| 46 | `set.UnionWith(other);` | Mutates a set to include values from both sets. |
| 47 | `Queue<string> jobs = new();` | Creates a first-in, first-out collection. |
| 48 | `jobs.Peek();` | Reads the next queue item without removing it. |
| 49 | `Stack<string> undo = new();` | Creates a last-in, first-out collection. |
| 50 | `undo.Peek();` | Reads the top stack item without popping it. |
| 51 | `ICollection<string> collection = new List<string>();` | Uses an interface that supports adding and counting. |
| 52 | `IReadOnlyCollection<string> readOnly = collection.ToList();` | Exposes count and enumeration without mutation methods. |
| 53 | `Dictionary<string, List<int>> grouped = new();` | Stores multiple values per key manually. |
| 54 | `var lookup = items.ToLookup(x => x.Category);` | Creates a multi-value lookup with LINQ. |
| 55 | `yield break;` | Ends an iterator early. |
| 56 | `Enumerable.Range(1, 10)` | Produces a sequence of numbers without writing a loop. |
| 57 | `Enumerable.Repeat("x", 3)` | Produces the same value several times. |
| 58 | `var immutable = numbers.AsReadOnly();` | Wraps a list in a read-only view. |
| 59 | `ConcurrentDictionary<string, int> counts = new();` | Provides thread-safe key/value operations. |
| 60 | `counts.AddOrUpdate("a", 1, (_, old) => old + 1);` | Adds or updates an entry thread-safely. Under contention the update delegate can run more than once, so keep it free of side effects. |
