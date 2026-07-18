# 08 — Collections

Collections hold multiple values. You'll use them in every program. This chapter covers arrays, `List<T>`, `Dictionary<TKey,TValue>`, sets, queues/stacks, the collection interfaces, and iterators. Knowing which collection to pick — and the Big-O cost of operations — is a real mid-level skill.

## Arrays — fixed size, fast, low-level

An array is a fixed-length, indexed sequence of one type. Once created, its length can't change.

```csharp
int[] numbers = new int[5];          // five zeros
numbers[0] = 10;                     // set by index (0-based)
int first = numbers[0];              // get by index
Console.WriteLine(numbers.Length);   // 5

int[] primes = { 2, 3, 5, 7, 11 };   // initialized inline
int[] more = new[] { 1, 2, 3 };      // type inferred

foreach (int n in primes) Console.WriteLine(n);

// Multi-dimensional (rare in business code):
int[,] grid = new int[3, 3];
grid[0, 1] = 5;
```

### Index and range operators (handy)

```csharp
int[] xs = { 10, 20, 30, 40, 50 };
int last = xs[^1];          // 50  — ^1 means "1 from the end"
int secondLast = xs[^2];    // 40
int[] middle = xs[1..4];    // {20, 30, 40} — range [start..end) end-exclusive
int[] firstTwo = xs[..2];   // {10, 20}
int[] fromTwo = xs[2..];    // {30, 40, 50}
```

Arrays are great when the size is fixed and known. For anything that grows or shrinks, use a `List<T>`.

## `List<T>` — the default resizable collection

`List<T>` is the workhorse. It's a dynamic array: indexable, fast to append, grows automatically. **When in doubt, use a `List<T>`.**

```csharp
using System.Collections.Generic;   // (auto-imported with ImplicitUsings)

var fruits = new List<string>();
fruits.Add("apple");
fruits.Add("banana");
fruits.AddRange(new[] { "cherry", "date" });

fruits.Count;                    // 4 (note: Count, not Length)
fruits[0];                       // "apple" (indexable)
fruits.Contains("banana");       // true
fruits.IndexOf("cherry");        // 2
fruits.Remove("apple");          // remove by value
fruits.RemoveAt(0);              // remove by index
fruits.Insert(0, "avocado");     // insert at index
fruits.Sort();                   // in-place sort
fruits.Reverse();
fruits.Clear();                  // empty it

// Initialize with values (collection initializer):
var nums = new List<int> { 1, 2, 3, 4, 5 };

// Collection expression (C# 12+) — works for lists, arrays, spans:
List<int> more = [1, 2, 3];
int[] arr = [4, 5, 6];
List<int> combined = [..nums, ..more];     // spread
```

### Performance intuition (know these)

| Operation | `List<T>` cost |
|---|---|
| Index access `list[i]` | O(1) — instant |
| Add at end | O(1) amortized |
| Insert/Remove in middle | O(n) — shifts elements |
| Contains / IndexOf | O(n) — linear scan |

If you do lots of "is X present?" lookups, a `List` is the wrong tool — use a `HashSet` or `Dictionary` (O(1) lookups).

## `Dictionary<TKey, TValue>` — key/value lookups

A dictionary maps keys to values with fast (O(1) average) lookups. Use it whenever you need to find things *by* something (id, name, code).

```csharp
var ages = new Dictionary<string, int>
{
    ["Ann"] = 30,
    ["Bob"] = 25
};

ages["Cy"] = 40;                 // add or update
int annAge = ages["Ann"];        // 30 — but THROWS if key missing!

// Safe lookups:
if (ages.TryGetValue("Bob", out int bobAge))     // idiomatic — no exception
    Console.WriteLine(bobAge);

ages.ContainsKey("Ann");         // true
ages.Remove("Bob");
ages.Count;

// GetValueOrDefault — return a default if missing:
int x = ages.GetValueOrDefault("Zoe", -1);       // -1

// Iterate:
foreach (var (name, age) in ages)
    Console.WriteLine($"{name}: {age}");

foreach (string key in ages.Keys) { }
foreach (int value in ages.Values) { }
```

**Gotchas:**
- `dict[key]` **throws** `KeyNotFoundException` if the key is missing on read. Use `TryGetValue` or `GetValueOrDefault` when the key might not exist.
- Assigning `dict[key] = value` adds or overwrites (never throws). `dict.Add(key, value)` throws if the key already exists.
- Keys must have correct `Equals`/`GetHashCode` — string/int/Guid work perfectly; records work (value equality); custom classes need correct overrides.

This is one of the most useful collections in CRUD work — grouping, caching, indexing data by id, counting occurrences, etc.

## `HashSet<T>` — unique items, fast membership

A set stores **distinct** values with O(1) `Contains`. Use it to dedupe or to answer "have I seen this?" quickly.

```csharp
var seen = new HashSet<string>();
seen.Add("a");          // true (added)
seen.Add("a");          // false (already present — no duplicate)
seen.Contains("a");     // true — O(1)

// Set operations:
var a = new HashSet<int> { 1, 2, 3 };
var b = new HashSet<int> { 2, 3, 4 };
a.IntersectWith(b);     // a -> {2, 3}
a.UnionWith(b);         // combine
a.ExceptWith(b);        // difference

// Dedupe a list:
var unique = new HashSet<string>(listWithDuplicates);
```

## `Queue<T>` and `Stack<T>`

```csharp
// Queue — FIFO (first in, first out)
var queue = new Queue<string>();
queue.Enqueue("first");
queue.Enqueue("second");
string next = queue.Dequeue();   // "first"
string peek = queue.Peek();      // look without removing

// Stack — LIFO (last in, first out)
var stack = new Stack<int>();
stack.Push(1);
stack.Push(2);
int top = stack.Pop();           // 2
```

Used for processing pipelines, undo histories, BFS/DFS traversals.

## The collection interfaces (program against these)

You usually *store* a `List<T>` but *expose* or *accept* an interface, so callers aren't tied to your choice and can't accidentally mutate things:

```
IEnumerable<T>        -- can be foreach'd / LINQ'd. The most general; read-forward only.
  └ ICollection<T>    -- + Count, Add, Remove, Contains
      └ IList<T>      -- + index access [i], Insert, RemoveAt
IReadOnlyCollection<T>-- Count, read-only
IReadOnlyList<T>      -- + indexable, read-only
IDictionary<K,V>      -- key/value
```

Guidelines:
- **Accept the least specific type you need.** A method that only iterates should take `IEnumerable<T>`.
- **Return read-only types** (`IReadOnlyList<T>`, `IEnumerable<T>`) when callers shouldn't mutate your internal collection.

```csharp
// Good: flexible input, protected output
public IReadOnlyList<Order> GetRecentOrders(IEnumerable<Order> all)
    => all.Where(o => o.Date > DateTime.UtcNow.AddDays(-7))
          .ToList();
```

## Choosing the right collection

| Need | Use |
|---|---|
| Ordered, resizable, index access | `List<T>` |
| Fixed size, max performance | array `T[]` |
| Look up values by a key | `Dictionary<TKey,TValue>` |
| Unique items / fast "contains" | `HashSet<T>` |
| FIFO processing | `Queue<T>` |
| LIFO processing | `Stack<T>` |
| Read-only public exposure | `IReadOnlyList<T>` / `IEnumerable<T>` |

## Iterators with `yield`

You can produce a sequence lazily with `yield return`, without building a full list in memory. The method returns `IEnumerable<T>` and runs on demand as you iterate.

```csharp
public static IEnumerable<int> Fibonacci(int count)
{
    int a = 0, b = 1;
    for (int i = 0; i < count; i++)
    {
        yield return a;            // hand out the next value, then pause here
        (a, b) = (b, a + b);
    }
}

foreach (int n in Fibonacci(10))
    Console.WriteLine(n);          // 0 1 1 2 3 5 8 13 21 34
```

`yield` enables **lazy evaluation**: values are computed one at a time as the consumer asks. This is exactly how LINQ works under the hood (next chapter). It lets you process huge or infinite sequences without materializing them all.

## A note on thread safety

The standard collections (`List`, `Dictionary`, etc.) are **not thread-safe**. If multiple threads mutate one concurrently you'll get corruption or exceptions. For concurrent scenarios use `System.Collections.Concurrent` types (`ConcurrentDictionary<,>`, `ConcurrentQueue<>`) or proper locking. You'll meet this when doing async/parallel work; just know plain collections aren't safe to share across threads for writes.

## Exercises

1. Read a list of words from the console (until blank), then print each unique word and how many times it appeared. (Hint: `Dictionary<string,int>`.)
2. Given a `List<int>`, produce a new list with duplicates removed but original order preserved. (Hint: `HashSet` to track seen.)
3. Write an iterator method `IEnumerable<int> EvenNumbers(int max)` using `yield`.
4. Build a phone-book `Dictionary<string,string>` with add/lookup/delete, using `TryGetValue` for safe lookups and a friendly "not found" message.

---

**Next:** [09 — Generics](./09-generics.md)
