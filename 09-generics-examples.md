# 09 - Generics Examples

These snippets reinforce generic methods, classes, constraints, result types, repositories, delegates, and variance.

## Example 1 - Generic identity method

```csharp
static T Identity<T>(T value) => value;
```

`T` is a type parameter, so the same method works with many types.

## Example 2 - Type inference

```csharp
int number = Identity(42);
string text = Identity("hello");
```

The compiler can often infer `T` from the argument you pass.

## Example 3 - Generic swap

```csharp
static void Swap<T>(ref T left, ref T right)
{
    (left, right) = (right, left);
}
```

Generic methods avoid duplicating the same logic for different types.

## Example 4 - Generic list helper

```csharp
static T First<T>(IReadOnlyList<T> items) => items[0];
```

The return type matches the element type of the collection.

## Example 5 - Generic class

```csharp
public class Box<T>
{
    public T Value { get; set; }
    public Box(T value) => Value = value;
}
```

Generic classes store or operate on a type chosen by the caller.

## Example 6 - Generic pair

```csharp
public class Pair<TFirst, TSecond>
{
    public Pair(TFirst first, TSecond second) => (First, Second) = (first, second);
    public TFirst First { get; }
    public TSecond Second { get; }
}
```

Multiple type parameters let one type connect several independent value types.

## Example 7 - Constraint to class

```csharp
static bool IsMissing<T>(T? value) where T : class
{
    return value is null;
}
```

The `class` constraint says `T` must be a reference type.

## Example 8 - Constraint to struct

```csharp
static T? Maybe<T>(bool include, T value) where T : struct
{
    return include ? value : null;
}
```

The `struct` constraint allows nullable value type syntax with `T?`.

## Example 9 - Constructor constraint

```csharp
static T Create<T>() where T : new()
{
    return new T();
}
```

The `new()` constraint allows generic code to call a public parameterless constructor.

## Example 10 - Interface constraint

```csharp
static void SaveAll<T>(IEnumerable<T> items) where T : ISaveable
{
    foreach (T item in items) item.Save();
}
```

Interface constraints let generic code call members from that interface.

## Example 11 - Generic result type

```csharp
public record Result<T>(bool Success, T? Value, string? Error);
```

A generic result can carry a value of any type plus success or failure information.

## Example 12 - Success factory

```csharp
static Result<T> Ok<T>(T value) => new(true, value, null);
```

Factory methods make generic result creation shorter and more consistent.

## Example 13 - Generic repository interface

```csharp
public interface IRepository<T>
{
    T? FindById(int id);
    void Add(T entity);
}
```

A repository interface can describe common storage operations for many entity types.

## Example 14 - In-memory repository

```csharp
public class MemoryRepository<T> : IRepository<T>
{
    private readonly List<T> _items = new();
    public T? FindById(int id) => default;
    public void Add(T entity) => _items.Add(entity);
}
```

Generic implementations can reuse storage logic while leaving entity details to callers.

## Example 15 - Func with generics

```csharp
static IEnumerable<TResult> Map<T, TResult>(IEnumerable<T> items, Func<T, TResult> selector)
{
    foreach (T item in items) yield return selector(item);
}
```

`Func<T, TResult>` lets callers provide transformation behavior.

## Example 16 - Action with generics

```csharp
static void ForEach<T>(IEnumerable<T> items, Action<T> action)
{
    foreach (T item in items) action(item);
}
```

`Action<T>` represents a method that consumes a value and returns nothing.

## Example 17 - Predicate with generics

```csharp
static IEnumerable<T> Keep<T>(IEnumerable<T> items, Predicate<T> predicate)
{
    foreach (T item in items)
        if (predicate(item)) yield return item;
}
```

`Predicate<T>` represents a true-or-false test for values of type `T`.

## Example 18 - Covariance example

```csharp
IEnumerable<string> names = new List<string>();
IEnumerable<object> objects = names;
```

Covariance lets an output-only generic interface use a more general type.

## Example 19 - Contravariance example

```csharp
Action<object> printObject = Console.WriteLine;
Action<string> printString = printObject;
```

Contravariance lets an input-only delegate accept a less specific type safely.

## Example 20 - Avoid pointless generics

```csharp
static string NormalizeName(string name) => name.Trim().ToUpperInvariant();
```

Use generics when the type truly varies; concrete methods are clearer when the type is fixed.

Helper type used above:

```csharp
public interface ISaveable
{
    void Save();
}
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `static T Last<T>(IReadOnlyList<T> items) => items[^1];` | Returns the same type as the list element type. |
| 22 | `static List<T> Singleton<T>(T value) => new() { value };` | Builds a typed list without knowing the type in advance. |
| 23 | `static bool Contains<T>(IEnumerable<T> items, T value)` | Generic search logic can work for any element type. |
| 24 | `public class Cache<TKey, TValue> where TKey : notnull` | Dictionary-style generic keys should not be nullable. |
| 25 | `private readonly Dictionary<TKey, TValue> _items = new();` | Generic fields can use the type parameters from the class. |
| 26 | `public void Set(TKey key, TValue value)` | Stores caller-chosen key and value types. |
| 27 | `public bool TryGet(TKey key, out TValue? value)` | A generic Try method can return a typed output value. |
| 28 | `where T : class` | Restricts a type parameter to reference types. |
| 29 | `where T : struct` | Restricts a type parameter to non-nullable value types. |
| 30 | `where T : unmanaged` | Restricts a type parameter to simple unmanaged value types. |
| 31 | `where T : Enum` | Restricts a type parameter to enum types. |
| 32 | `where T : notnull` | Prevents nullable type arguments. |
| 33 | `where T : IComparable<T>` | Allows generic code to compare values. |
| 34 | `static T Max<T>(T a, T b) where T : IComparable<T>` | Uses a constraint to call `CompareTo`. |
| 35 | `public interface IReadRepository<out T>` | Covariance is useful for producers of `T`. |
| 36 | `public interface IHandler<in T>` | Contravariance is useful for consumers of `T`. |
| 37 | `Func<string, int> length = s => s.Length;` | `Func` is a generic delegate with a return value. |
| 38 | `Action<string> print = Console.WriteLine;` | `Action` is a generic delegate with no return value. |
| 39 | `Predicate<int> positive = n => n > 0;` | `Predicate<T>` represents a boolean test. |
| 40 | `Comparer<T>.Default` | Gets the default comparer for a generic type. |
| 41 | `EqualityComparer<T>.Default` | Gets the default equality logic for a generic type. |
| 42 | `default(T)` | Produces the default value for the current type parameter. |
| 43 | `T? value` | Nullable generic syntax depends on constraints and nullable context. |
| 44 | `List<int>` | A closed generic type with `T` replaced by `int`. |
| 45 | `Dictionary<string, decimal>` | A generic type with two concrete type arguments. |
| 46 | `Task<string>` | Async APIs use generics to represent result types. |
| 47 | `Nullable<int>` | The long form behind `int?`. |
| 48 | `IEnumerable<T>` | The standard generic sequence interface. |
| 49 | `IReadOnlyDictionary<TKey, TValue>` | A read-only generic key/value contract. |
| 50 | `public record Page<T>(IReadOnlyList<T> Items, int Total);` | Generic records are useful for reusable response shapes. |
| 51 | `public record ErrorOr<T>(T? Value, string? Error);` | Generic wrappers carry typed values and shared metadata. |
| 52 | `static Result<T> Fail<T>(string error)` | Generic factories create result values without knowing `T`. |
| 53 | `public class Option<T>` | Option types model present-or-missing values generically. |
| 54 | `public T Value { get; }` | Generic properties preserve the caller's chosen type. |
| 55 | `public static Box<T> Create(T value)` | Static factories can be generic or use class type parameters. |
| 56 | `typeof(T)` | Gets the runtime `Type` object for the current type parameter. |
| 57 | `value is T typed` | Pattern matching can test whether an object is compatible with `T`. |
| 58 | `Array.Empty<T>()` | Returns a cached empty array for any element type. |
| 59 | `Enumerable.Empty<T>()` | Returns an empty sequence of a chosen type. |
| 60 | `new Stack<T>()` | Generic collections let you choose the element type once. |
