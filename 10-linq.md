# 10 — LINQ

**LINQ** (Language Integrated Query) is how you filter, transform, sort, group, and aggregate data in C#. It is, no exaggeration, one of the most-used features in real C# work — you'll write it dozens of times a day. The same operators work over in-memory collections *and* over databases (via EF Core, Chapter 19). Learn this chapter cold.

LINQ methods are extension methods on `IEnumerable<T>` living in `System.Linq` (auto-imported via ImplicitUsings).

## Two syntaxes (you can ignore one)

LINQ has **method syntax** (fluent, chained calls) and **query syntax** (SQL-like keywords). They compile to the same thing. **Method syntax is far more common in the wild** — learn it as your primary. We'll show query syntax briefly so you recognize it.

```csharp
var nums = new[] { 5, 3, 8, 1, 9, 2 };

// Method syntax (use this):
var result = nums.Where(n => n > 3).OrderBy(n => n).ToList();

// Query syntax (recognize it):
var result2 = (from n in nums
               where n > 3
               orderby n
               select n).ToList();
```

Both give `[5, 8, 9]`. We'll use method syntax throughout.

## The mental model

Every LINQ operator takes a sequence and returns a new sequence (or a single value). You **chain** them into a pipeline. The lambda `n => n > 3` is a function applied to each element (lambdas are covered fully in Chapter 12; for now: `parameter => expression`).

```csharp
people
  .Where(p => p.Age >= 18)        // keep adults
  .OrderBy(p => p.LastName)       // sort by last name
  .Select(p => p.Email)           // project to just emails
  .ToList();                      // materialize into a List<string>
```

## Filtering: `Where`

```csharp
var adults = people.Where(p => p.Age >= 18);
var active = users.Where(u => u.IsActive && u.LastLogin > cutoff);
var nonEmpty = names.Where(n => !string.IsNullOrWhiteSpace(n));
```

`Where` keeps elements for which the lambda returns `true`.

## Projection: `Select` and `SelectMany`

`Select` transforms each element into something else (like `map` in other languages):

```csharp
var names = people.Select(p => p.Name);                  // IEnumerable<string>
var upper = names.Select(n => n.ToUpper());
var dtos = people.Select(p => new PersonDto(p.Name, p.Age)); // map entity -> DTO

// Project to an anonymous type (a throwaway shape):
var summary = people.Select(p => new { p.Name, IsAdult = p.Age >= 18 });
foreach (var x in summary) Console.WriteLine($"{x.Name}: {x.IsAdult}");

// Select with index:
var numbered = names.Select((name, i) => $"{i + 1}. {name}");
```

`SelectMany` flattens nested sequences into one:

```csharp
var orders = customers.SelectMany(c => c.Orders);   // all orders across all customers
var allTags = posts.SelectMany(p => p.Tags).Distinct();
```

## Sorting: `OrderBy`, `ThenBy`, `OrderByDescending`

```csharp
var sorted = people.OrderBy(p => p.LastName)
                   .ThenBy(p => p.FirstName);        // tie-breaker

var newestFirst = posts.OrderByDescending(p => p.CreatedAt);

var byAgeThenName = people
    .OrderByDescending(p => p.Age)
    .ThenBy(p => p.Name);
```

`OrderBy` ascending, `OrderByDescending` descending, `ThenBy`/`ThenByDescending` for secondary keys. (Don't chain two `OrderBy`s — the second wins; use `ThenBy`.)

## Getting single elements: First / Single / Last / ElementAt

```csharp
var first = people.First();                       // first element — throws if empty
var firstAdult = people.First(p => p.Age >= 18);  // first matching — throws if none
var firstOrNull = people.FirstOrDefault(p => p.Age >= 18);  // null/default if none

var only = people.Single(p => p.Id == 5);         // exactly one — throws if 0 or 2+
var onlyOrNull = people.SingleOrDefault(p => p.Id == 5);    // null if 0; throws if 2+

var last = people.LastOrDefault();
```

**Critical distinction:**
- `First` / `FirstOrDefault` — "give me the first one (that matches)."
- `Single` / `SingleOrDefault` — "there should be exactly one; complain if there are more." Use for lookups by a unique key where 2+ matches indicates a bug.
- The `...OrDefault` variants return `default(T)` (null for reference types) instead of throwing when nothing matches. **Always consider whether empty is possible** and use `OrDefault` + a null check, or you'll get exceptions in production.

## Testing: Any / All / Contains

```csharp
bool hasAdults = people.Any(p => p.Age >= 18);    // is there at least one?
bool anyAtAll = people.Any();                     // is the sequence non-empty?
bool allAdults = people.All(p => p.Age >= 18);    // do all match?
bool hasBob = names.Contains("Bob");
```

`Any()` is the idiomatic "is it non-empty?" check — clearer than `Count() > 0`, and never slower: `Count()` is only O(1) when the source is a collection that already knows its count (e.g. `List<T>`, arrays); on a lazy query or a database query it walks or counts everything.

## Counting and aggregation

```csharp
int count = people.Count();
int adultCount = people.Count(p => p.Age >= 18);  // count matching

decimal total = orders.Sum(o => o.Amount);
decimal avg = orders.Average(o => o.Amount);
decimal max = orders.Max(o => o.Amount);
decimal min = orders.Min(o => o.Amount);

var oldest = people.MaxBy(p => p.Age);            // the element with the max age
var cheapest = products.MinBy(p => p.Price);      // (MaxBy/MinBy return the element)

// Aggregate — general-purpose fold (rarely needed; the above cover most cases):
int product = nums.Aggregate(1, (acc, n) => acc * n);
```

`MaxBy`/`MinBy` give you the *element* with the extreme value (vs `Max`/`Min` which give the value itself) — very handy.

## Grouping: `GroupBy`

Group elements by a key — extremely common for reports and summaries:

```csharp
var byCity = people.GroupBy(p => p.City);

foreach (var group in byCity)
{
    Console.WriteLine($"{group.Key}: {group.Count()} people");
    foreach (var person in group)
        Console.WriteLine($"  {person.Name}");
}

// Group + aggregate into a summary (super common):
var salesByRegion = orders
    .GroupBy(o => o.Region)
    .Select(g => new
    {
        Region = g.Key,
        Total = g.Sum(o => o.Amount),
        Count = g.Count()
    })
    .OrderByDescending(x => x.Total)
    .ToList();
```

Each `group` is itself an `IEnumerable<T>` with a `.Key`.

## Converting keyed data: `ToDictionary` / `ToLookup`

```csharp
// Index a list by id for O(1) lookups:
Dictionary<int, Customer> byId = customers.ToDictionary(c => c.Id);
var customer = byId[42];

// ToDictionary with a value selector:
Dictionary<int, string> idToName = customers.ToDictionary(c => c.Id, c => c.Name);
```

(`ToDictionary` throws on duplicate keys; `ToLookup` allows multiple values per key.)

## Joining two sequences: `Join`

```csharp
var orderInfo = orders.Join(
    customers,                       // the other sequence
    order => order.CustomerId,       // key from orders
    cust => cust.Id,                 // key from customers
    (order, cust) => new { cust.Name, order.Amount });  // result selector
```

In practice you join far more often in EF Core queries (against the DB) than in memory, but the operator is the same.

## Set-like and partitioning operators

```csharp
nums.Distinct();                 // remove duplicates
nums.DistinctBy(x => x.Id);      // distinct by a key
a.Union(b);                      // set union (distinct)
a.Intersect(b);                  // common elements
a.Except(b);                     // in a but not b

nums.Take(3);                    // first 3
nums.Skip(2);                    // all but first 2
nums.Take(2..5);                 // range-based take
nums.TakeWhile(n => n < 5);      // take until predicate fails
nums.SkipWhile(n => n < 5);
nums.Chunk(100);                 // split into batches of 100 (great for bulk ops)
nums.Reverse();
```

> **C# 14 / .NET 10 gotcha:** when `nums` is an **array**, `nums.Reverse()` now binds to `MemoryExtensions.Reverse(Span<T>)`, which reverses the array *in place* and returns nothing — not LINQ's lazy `Enumerable.Reverse`. If you want a reversed sequence without touching the array, call `Enumerable.Reverse(nums)` explicitly.

`Skip`/`Take` are how you implement **pagination**: `query.Skip((page - 1) * size).Take(size)`.

## Materializing: `ToList`, `ToArray`, and deferred execution

This is the concept that bites people. **Most LINQ operators are lazy (deferred):** they don't run when you call them — they run when you *enumerate* the result (foreach, `ToList`, etc.).

```csharp
var query = numbers.Where(n => { Console.WriteLine($"checking {n}"); return n > 2; });
// Nothing printed yet! The Where hasn't executed.

var list = query.ToList();   // NOW it runs and prints
```

Consequences:
- **Re-enumerating runs the query again.** If `people` is a database query, iterating it twice hits the DB twice.
- A deferred query reflects the *current* state of the source when enumerated, not when defined.

```csharp
var source = new List<int> { 1, 2, 3 };
var evens = source.Where(n => n % 2 == 0);   // deferred
source.Add(4);
foreach (var n in evens) Console.Write(n);    // prints 2 4 — sees the added 4!
```

**Materialize with `ToList()`/`ToArray()`** when you want to:
- Iterate the result multiple times without recomputing.
- "Freeze" results before the source changes.
- Force a database query to execute at a known point.

Rule of thumb: build your pipeline lazily, then `.ToList()` once at the end when you need the concrete results. Don't sprinkle `ToList()` mid-pipeline (it allocates and forces evaluation early).

## Putting it together — realistic queries

```csharp
// Top 5 customers by total spend in the last 30 days
var topCustomers = orders
    .Where(o => o.Date >= DateTime.UtcNow.AddDays(-30))
    .GroupBy(o => o.CustomerId)
    .Select(g => new { CustomerId = g.Key, Spend = g.Sum(o => o.Amount) })
    .OrderByDescending(x => x.Spend)
    .Take(5)
    .ToList();

// Paginated, filtered, sorted list for an API endpoint
var page = products
    .Where(p => p.IsActive && p.Price <= maxPrice)
    .OrderBy(p => p.Name)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .Select(p => new ProductDto(p.Id, p.Name, p.Price))
    .ToList();

// Validation: are all line items in stock?
bool canFulfill = order.Lines.All(line => inventory[line.Sku] >= line.Quantity);
```

## Common mistakes (avoid these)

1. **Calling `.Count() > 0` instead of `.Any()`** — `Any()` stops at the first element; `Count()` walks the whole sequence unless the source is a collection with a stored count.
2. **`First()` when the sequence might be empty** — use `FirstOrDefault()` and null-check, or you get `InvalidOperationException`.
3. **Enumerating an expensive/DB query multiple times** — `ToList()` once and reuse.
4. **Doing work inside `Select` that has side effects** — keep projections pure; LINQ may run them lazily or repeatedly.
5. **Forgetting that `OrderBy` returns a new sequence** — LINQ never mutates the source; you must use the return value.

## Exercises

1. Given a `List<Order>` (with `CustomerId`, `Amount`, `Date`), compute total revenue, average order value, and the highest single order.
2. Group a list of people by the first letter of their last name and print each group sorted.
3. Implement pagination: write `IReadOnlyList<T> GetPage<T>(IEnumerable<T> source, int page, int size)`.
4. From a list of products, return the names of the 3 most expensive *active* products, uppercased.
5. Demonstrate deferred execution: build a `Where` query over a list, add an item to the list, then enumerate, and explain the output.

---

**Next:** [11 — Exceptions & Resources](./11-exceptions-and-resources.md)
