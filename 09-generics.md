# 09 — Generics

Generics let you write code that works with **any type** while staying fully type-safe. You've already used them — `List<T>`, `Dictionary<TKey,TValue>`, `Task<T>`. This chapter shows you how to *write* generic types and methods, and use constraints. Mid-level devs read generic code daily and write it occasionally (repositories, result wrappers, utility methods).

## The problem generics solve

Before generics, reusable containers used `object`, which lost type safety and caused boxing:

```csharp
// Bad old way:
var list = new System.Collections.ArrayList();
list.Add(5);
list.Add("oops");                 // no error — accepts anything
int x = (int)list[0];             // must cast everywhere, can blow up at runtime
```

Generics fix this — one implementation, type-checked per use:

```csharp
var list = new List<int>();
list.Add(5);
// list.Add("oops");              // compile error — caught immediately
int x = list[0];                  // no cast needed
```

`T` is a **type parameter** — a placeholder filled in when you use the type. `List<int>`, `List<string>`, `List<Customer>` are all the same code specialized to a type, with no casting and no boxing.

## Generic methods

A method can have its own type parameter(s) in angle brackets after its name:

```csharp
public static T Max<T>(T a, T b) where T : IComparable<T>
    => a.CompareTo(b) > 0 ? a : b;

int m = Max(3, 7);                 // T inferred as int -> 7
string s = Max("apple", "pear");   // T inferred as string -> "pear"

// Swap two values of any type:
public static void Swap<T>(ref T a, ref T b)
{
    T temp = a;
    a = b;
    b = temp;
}
```

Usually you don't specify `<T>` at the call site — the compiler infers it from the arguments. You can be explicit when needed: `Max<int>(3, 7)`.

## Generic classes

Define a reusable type parameterized by `T`. Classic example — a simple stack:

```csharp
public class Box<T>
{
    private T? _item;
    public bool HasValue { get; private set; }

    public void Put(T item)
    {
        _item = item;
        HasValue = true;
    }

    public T Get()
    {
        if (!HasValue) throw new InvalidOperationException("Box is empty");
        return _item!;
    }
}

var intBox = new Box<int>();
intBox.Put(42);
int v = intBox.Get();

var nameBox = new Box<string>();
nameBox.Put("hello");
```

### Multiple type parameters

```csharp
public class Pair<TKey, TValue>
{
    public TKey Key { get; init; } = default!;
    public TValue Value { get; init; } = default!;
}

var p = new Pair<string, int> { Key = "age", Value = 30 };
```

`default(T)` / `default` gives the type's default value — `0` for numbers, `false` for bool, `null` for reference types. The `!` after it is the null-forgiving operator (Chapter 15).

## Constraints — restricting `T`

By default `T` could be *anything*, so you can only use `object` members on it. **Constraints** (`where`) let you require capabilities, unlocking more operations:

```csharp
// T must implement IComparable<T> -> can call CompareTo:
public static T Min<T>(T a, T b) where T : IComparable<T>
    => a.CompareTo(b) < 0 ? a : b;

// T must be a class (reference type):
public class Cache<T> where T : class { }

// T must be a struct (value type):
public class Buffer<T> where T : struct { }

// T must have a public parameterless constructor -> can 'new' it:
public static T Create<T>() where T : new() => new T();

// T must derive from / implement a specific type:
public class Repository<T> where T : Entity { }

// Combine multiple constraints:
public class Store<T> where T : Entity, IComparable<T>, new() { }
```

Common constraint keywords:
- `where T : class` — reference type
- `where T : struct` — value type (non-nullable)
- `where T : new()` — has a parameterless constructor
- `where T : SomeBaseClass` — derives from it
- `where T : ISomeInterface` — implements it
- `where T : notnull` — not nullable

Constraints are how you write generic code that actually *does* something with `T` beyond storing it.

## A realistic example: a generic Result type

You'll often see a generic "result" wrapper to represent success-or-failure without exceptions. This shows generics in everyday use:

```csharp
public class Result<T>
{
    public bool IsSuccess { get; private init; }
    public T? Value { get; private init; }
    public string? Error { get; private init; }

    public static Result<T> Success(T value)
        => new() { IsSuccess = true, Value = value };

    public static Result<T> Failure(string error)
        => new() { IsSuccess = false, Error = error };
}

// Usage:
Result<Customer> LoadCustomer(int id)
{
    var customer = _db.Find(id);
    return customer is null
        ? Result<Customer>.Failure($"No customer {id}")
        : Result<Customer>.Success(customer);
}

var result = LoadCustomer(5);
if (result.IsSuccess)
    Console.WriteLine(result.Value!.Name);
else
    Console.WriteLine(result.Error);
```

## A realistic example: a generic repository

The "repository pattern" wraps data access. A generic base avoids repeating CRUD for every entity type — you'll see this constantly:

```csharp
public interface IRepository<T> where T : class
{
    T? GetById(int id);
    IReadOnlyList<T> GetAll();
    void Add(T entity);
    void Remove(T entity);
}

public class InMemoryRepository<T> : IRepository<T> where T : class
{
    private readonly List<T> _items = new();
    public T? GetById(int id) => /* lookup logic */ default;
    public IReadOnlyList<T> GetAll() => _items;
    public void Add(T entity) => _items.Add(entity);
    public void Remove(T entity) => _items.Remove(entity);
}

IRepository<Customer> customers = new InMemoryRepository<Customer>();
IRepository<Order> orders = new InMemoryRepository<Order>();
```

One implementation, reused for every entity type, fully type-safe. (Real apps usually back this with EF Core — Chapter 19.)

## `Func<>`, `Action<>` and generics

The built-in delegate types are generic and you'll use them everywhere (full treatment in Chapter 12):

```csharp
Func<int, int, int> add = (a, b) => a + b;   // takes two ints, returns int
Action<string> log = msg => Console.WriteLine(msg);  // takes a string, returns void
Predicate<int> isEven = n => n % 2 == 0;             // takes T, returns bool
```

LINQ is built on these generic delegates.

## Covariance & contravariance (just enough)

This is the one genuinely advanced generics topic. You mostly need to *recognize* it, not write it:

- **Covariance** (`out`): `IEnumerable<Dog>` can be assigned to `IEnumerable<Animal>` because you only read out `Animal`s. That's why this works:

```csharp
IEnumerable<Dog> dogs = GetDogs();
IEnumerable<Animal> animals = dogs;   // OK — IEnumerable<out T> is covariant
```

- **Contravariance** (`in`): an `IComparer<Animal>` can be used where an `IComparer<Dog>` is expected.

The BCL's `IEnumerable<out T>`, `IReadOnlyList<out T>`, `Func<>` return types, and `Action<>` parameters are set up for this. You rarely declare your own variant interfaces; just understand why assigning `IEnumerable<Derived>` to `IEnumerable<Base>` compiles.

## When to write generics (and when not to)

**Write a generic** when you have genuinely type-agnostic logic repeated for multiple types: containers, repositories, result/option wrappers, caching, mapping utilities.

**Don't over-generify.** If something is only ever used with one type, a generic adds noise. Start concrete; extract a generic when the duplication is real. Premature generics make code harder to read for no benefit.

## Exercises

1. Write a generic method `IEnumerable<T> Repeat<T>(T value, int count)`.
2. Implement `Stack<T>` from scratch (Push/Pop/Peek/Count) backed by a `List<T>`. Throw on Pop/Peek when empty.
3. Write `Result<T>` (success/failure) as above and a method that uses it to parse an int safely.
4. Write a generic `T[] WhereMatches<T>(T[] items, Predicate<T> match)` that returns the matching elements (don't use LINQ — implement it).

---

**Next:** [10 — LINQ](./10-linq.md)
