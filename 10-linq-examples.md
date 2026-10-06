# 10 - LINQ Examples

These snippets reinforce filtering, projection, sorting, grouping, joins, aggregation, set operations, and deferred execution.

## Example 1 - Where

```csharp
var numbers = new[] { 1, 2, 3, 4 };
var evens = numbers.Where(n => n % 2 == 0);
```

`Where` keeps only the items that match a predicate.

## Example 2 - Select

```csharp
var names = new[] { "Ada", "Grace" };
var lengths = names.Select(name => name.Length);
```

`Select` transforms each item into a new value.

## Example 3 - SelectMany

```csharp
var groups = new[] { new[] { 1, 2 }, new[] { 3, 4 } };
var flattened = groups.SelectMany(group => group);
```

`SelectMany` flattens nested sequences into one sequence.

## Example 4 - OrderBy

```csharp
var sorted = names.OrderBy(name => name);
```

`OrderBy` sorts ascending by the selected key.

## Example 5 - ThenBy

```csharp
var people = new[] { ("Ada", 36), ("Ada", 28), ("Grace", 85) };
var sorted = people.OrderBy(p => p.Item1).ThenBy(p => p.Item2);
```

`ThenBy` adds a secondary sort after the first ordering.

## Example 6 - First

```csharp
int firstEven = numbers.First(n => n % 2 == 0);
```

`First` returns the first match and throws if no match exists.

## Example 7 - FirstOrDefault

```csharp
int missing = numbers.FirstOrDefault(n => n > 100);
```

`FirstOrDefault` returns the default value when no item matches.

## Example 8 - Single

```csharp
var onlyAdmin = users.Single(user => user.Role == "Admin");
```

`Single` is for cases where exactly one matching item should exist.

## Example 9 - Any

```csharp
bool hasNegative = numbers.Any(n => n < 0);
```

`Any` answers whether at least one item matches.

## Example 10 - All

```csharp
bool allPositive = numbers.All(n => n > 0);
```

`All` answers whether every item matches.

## Example 11 - Count

```csharp
int passing = scores.Count(score => score >= 70);
```

`Count` can count every item or only items matching a predicate.

## Example 12 - Sum and Average

```csharp
decimal total = orders.Sum(order => order.Total);
decimal average = orders.Average(order => order.Total);
```

Aggregation methods reduce a sequence to summary values.

## Example 13 - GroupBy

```csharp
var byRole = users.GroupBy(user => user.Role);
```

`GroupBy` creates groups of items that share the same key.

## Example 14 - Reading groups

```csharp
foreach (var group in byRole)
{
    Console.WriteLine($"{group.Key}: {group.Count()}");
}
```

Each group has a key and can be enumerated like a sequence.

## Example 15 - ToDictionary

```csharp
var byId = users.ToDictionary(user => user.Id);
```

`ToDictionary` materializes a lookup where each key must be unique.

## Example 16 - ToLookup

```csharp
var ordersByCustomer = orders.ToLookup(order => order.CustomerId);
```

`ToLookup` allows multiple values per key.

## Example 17 - Join

```csharp
var query = customers.Join(
    orders,
    customer => customer.Id,
    order => order.CustomerId,
    (customer, order) => new { customer.Name, order.Total });
```

`Join` matches items from two sequences by related keys.

## Example 18 - Distinct

```csharp
var uniqueNames = names.Distinct();
```

`Distinct` removes duplicate values based on equality.

## Example 19 - Take and Skip

```csharp
var pageTwo = items.Skip(10).Take(10);
```

`Skip` and `Take` are common for paging through ordered data.

## Example 20 - Deferred execution

```csharp
var source = new List<int> { 1, 2, 3, 4 };
var query = source.Where(n => n > 2);
source.Add(10);
Console.WriteLine(string.Join(", ", query)); // 3, 4, 10
```

`Where` runs when the query is enumerated, so it sees the item added afterwards. Note what it does *not* see: reassigning the variable (`source = new List<int> { 99 };`) would still print `3, 4, 10`, because the query captured the original list object, not the variable.

Helper records used above:

```csharp
public record User(int Id, string Name, string Role);
public record Order(int CustomerId, decimal Total);
public record Customer(int Id, string Name);
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `items.Where(x => x.Active)` | Filters a sequence to active items. |
| 22 | `items.Where((x, index) => index < 10)` | Uses the overload that includes the item index. |
| 23 | `items.Select(x => x.Name)` | Projects objects into one property. |
| 24 | `items.Select((x, index) => new { index, x.Name })` | Includes indexes while projecting. |
| 25 | `orders.SelectMany(o => o.Lines)` | Flattens child collections from many parent objects. |
| 26 | `items.OrderBy(x => x.Name, StringComparer.OrdinalIgnoreCase)` | Sorts strings with an explicit comparer. |
| 27 | `items.OrderByDescending(x => x.CreatedAt)` | Sorts newest items first. |
| 28 | `items.Reverse()` | Reverses the sequence order lazily. If `items` is an array, C# 14 binds this to the in-place span `Reverse` instead — see the note in Chapter 10. |
| 29 | `items.FirstOrDefault()` | Returns the first item or the default value. |
| 30 | `items.SingleOrDefault(x => x.Id == id)` | Expects zero or one matching item. |
| 31 | `items.Last(x => x.Active)` | Finds the last active item. |
| 32 | `items.ElementAt(2)` | Reads an item by sequence position. |
| 33 | `items.ElementAtOrDefault(99)` | Avoids throwing when an index is out of range. |
| 34 | `items.Any()` | Checks whether a sequence contains at least one item. |
| 35 | `items.Any(x => x.Active)` | Checks whether any item matches a condition. |
| 36 | `items.All(x => x.Active)` | Checks whether every item matches a condition. |
| 37 | `items.Contains(target)` | Checks for a specific value using equality. |
| 38 | `items.Count()` | Counts items in a sequence. |
| 39 | `items.LongCount()` | Counts into a `long` for very large sequences. |
| 40 | `items.Min(x => x.Price)` | Finds the smallest projected value. |
| 41 | `items.Max(x => x.Price)` | Finds the largest projected value. |
| 42 | `items.Sum(x => x.Price)` | Adds projected numeric values. |
| 43 | `items.Average(x => x.Score)` | Calculates the mean of projected numeric values. |
| 44 | `items.Aggregate(0, (sum, x) => sum + x.Count)` | Builds a custom aggregation. |
| 45 | `items.GroupBy(x => x.Category)` | Groups items by a key. |
| 46 | `items.GroupBy(x => x.Category).Select(g => new { g.Key, Count = g.Count() })` | Summarizes each group. |
| 47 | `items.ToList()` | Materializes a query into a list immediately. |
| 48 | `items.ToArray()` | Materializes a query into an array. |
| 49 | `items.ToHashSet()` | Materializes unique values into a set. |
| 50 | `items.Distinct()` | Removes duplicates. |
| 51 | `items.DistinctBy(x => x.Email)` | Removes duplicates by a selected key. |
| 52 | `items.Union(other)` | Combines unique values from two sequences. |
| 53 | `items.Intersect(other)` | Keeps only values found in both sequences. |
| 54 | `items.Except(other)` | Removes values found in another sequence. |
| 55 | `items.Skip(20).Take(10)` | Reads a page of ten items after skipping twenty. |
| 56 | `items.TakeWhile(x => x.Active)` | Takes items until the condition first fails. |
| 57 | `items.SkipWhile(x => x.Hidden)` | Skips items until the condition first fails. |
| 58 | `items.Chunk(3)` | Splits a sequence into arrays of a fixed size. |
| 59 | `items.Zip(other, (a, b) => (a, b))` | Pairs items from two sequences by position. |
| 60 | `query.AsEnumerable()` | Switches to LINQ-to-Objects for later operations. |
