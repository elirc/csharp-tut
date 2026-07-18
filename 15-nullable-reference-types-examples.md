# 15 - Nullable Reference Types Examples

These snippets reinforce nullable annotations, flow analysis, null operators, API design, initialization, and adoption habits.

## Example 1 - Non-nullable string

```csharp
string name = "Ada";
```

With nullable reference types enabled, `string` means the variable should not contain null.

## Example 2 - Nullable string

```csharp
string? nickname = null;
```

`string?` tells the compiler and readers that null is an expected value.

## Example 3 - Flow analysis after null check

```csharp
if (nickname is not null)
{
    Console.WriteLine(nickname.Length);
}
```

The compiler understands that `nickname` is non-null inside the `if` block.

## Example 4 - Null conditional

```csharp
int? length = nickname?.Length;
```

`?.` returns null instead of accessing a member on a null reference.

## Example 5 - Null coalescing

```csharp
string displayName = nickname ?? "Guest";
```

`??` replaces null with a fallback value.

## Example 6 - Null coalescing assignment

```csharp
List<string>? names = null;
names ??= new List<string>();
```

`??=` initializes a variable only when it is currently null.

## Example 7 - Null forgiving

```csharp
string input = Console.ReadLine()!;
```

`!` suppresses a nullable warning, but it does not add a runtime check.

## Example 8 - Prefer validation over forgiving

```csharp
string? input = Console.ReadLine();
if (input is null) return;
Console.WriteLine(input.Length);
```

Checking for null is safer than silencing the compiler when null is possible.

## Example 9 - Nullable parameter

```csharp
static void PrintOptional(string? message)
{
    Console.WriteLine(message ?? "(none)");
}
```

Nullable parameters tell callers they may pass null.

## Example 10 - Non-nullable parameter

```csharp
static void PrintRequired(string message)
{
    ArgumentNullException.ThrowIfNull(message);
    Console.WriteLine(message);
}
```

Non-nullable parameters document intent, and runtime validation protects public APIs.

## Example 11 - Nullable return

```csharp
static string? FindName(int id)
{
    return id == 1 ? "Ada" : null;
}
```

A nullable return type forces callers to handle the missing case.

## Example 12 - Try pattern avoids nullable return

```csharp
static bool TryFindName(int id, out string name)
{
    name = id == 1 ? "Ada" : "";
    return id == 1;
}
```

The `Try` pattern can communicate success separately from the returned value.

## Example 13 - Required initialization

```csharp
public class User
{
    public required string Email { get; init; }
}
```

`required` prevents callers from creating an object without important non-null data.

## Example 14 - Constructor initialization

```csharp
public class Project
{
    public Project(string name) => Name = name;
    public string Name { get; }
}
```

Constructors are a strong way to guarantee non-null members are initialized.

## Example 15 - Default empty collection

```csharp
public class Team
{
    public List<string> Members { get; } = new();
}
```

Empty collections are often better than nullable collections because callers can enumerate safely.

## Example 16 - Nullable value type

```csharp
int? optionalAge = null;
```

Nullable value types existed before nullable reference types and represent optional values.

## Example 17 - HasValue

```csharp
if (optionalAge.HasValue)
{
    Console.WriteLine(optionalAge.Value);
}
```

`HasValue` checks whether a nullable value type contains a value.

## Example 18 - Pattern with nullable value type

```csharp
if (optionalAge is int age)
{
    Console.WriteLine(age);
}
```

Pattern matching unwraps the nullable value only when it exists.

## Example 19 - Enable nullable in project

```xml
<Nullable>enable</Nullable>
```

This project setting turns on nullable reference type analysis.

## Example 20 - Narrow scope during adoption

```csharp
#nullable enable
string? value = null;
#nullable disable
```

Nullable directives can help migrate older code gradually.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `string title = "Hello";` | A non-nullable reference should always contain a value. |
| 22 | `string? title = null;` | A nullable reference explicitly allows null. |
| 23 | `title!.Length` | Suppresses a warning but does not prevent runtime null failure. |
| 24 | `title?.Length` | Safely returns null when `title` is null. |
| 25 | `title ?? "Untitled"` | Replaces null with a fallback value. |
| 26 | `title ??= "Untitled"` | Stores the fallback for later use too. |
| 27 | `if (title is null) return;` | Guards against null before continuing. |
| 28 | `if (title is not null) Use(title);` | Narrows a nullable variable to non-null in the block. |
| 29 | `ArgumentNullException.ThrowIfNull(title);` | Performs a runtime null check and helps flow analysis. |
| 30 | `public string Name { get; set; } = "";` | Initializes a non-nullable property with an empty default. |
| 31 | `public required string Name { get; init; }` | Requires object initializers to provide a value. |
| 32 | `public User(string name) => Name = name;` | Constructor assignment satisfies non-nullable initialization. |
| 33 | `public List<string> Tags { get; } = new();` | Empty collections often avoid nullable collection properties. |
| 34 | `public string? MiddleName { get; init; }` | Nullable properties are appropriate for truly optional data. |
| 35 | `static string? Find(int id)` | A nullable return forces callers to handle missing values. |
| 36 | `static bool TryFind(int id, out string value)` | Try patterns can avoid nullable returns. |
| 37 | `[NotNullWhen(true)] out string? value` | Attributes can teach flow analysis about custom Try methods. |
| 38 | `[MaybeNull] T Find<T>()` | Attributes can express generic nullability behavior. |
| 39 | `IEnumerable<string?>` | Means the sequence itself is non-null but elements may be null. |
| 40 | `IEnumerable<string>?` | Means the sequence reference may be null. |
| 41 | `IEnumerable<string?>?` | Means both the sequence and its elements may be null. |
| 42 | `Dictionary<string, string?>` | Values may be null even though keys are non-null strings. |
| 43 | `string?[] names` | Array elements may be null. |
| 44 | `int? age = null;` | Nullable value types represent optional value data. |
| 45 | `age is int actualAge` | Pattern matching unwraps a nullable value type. |
| 46 | `age.GetValueOrDefault(-1)` | Reads a nullable value type with a custom fallback. |
| 47 | `<Nullable>enable</Nullable>` | Enables nullable analysis for the project. |
| 48 | `#nullable disable` | Turns nullable analysis off for a region or file. |
| 49 | `#nullable restore` | Restores the project-level nullable setting. |
| 50 | `string? input = Console.ReadLine();` | Many BCL APIs correctly expose possibly-null values. |
| 51 | `if (string.IsNullOrWhiteSpace(input)) return;` | Common string checks narrow nullability. |
| 52 | `input.Trim()` | Safe after the compiler knows input is not null. |
| 53 | `object? value = GetValue();` | Unknown data should often be modeled as nullable. |
| 54 | `if (value is string text) Use(text);` | Type patterns both check type and non-nullness. |
| 55 | `T? Maybe<T>() where T : class` | Nullable generic references usually need constraints. |
| 56 | `T? MaybeValue<T>() where T : struct` | Nullable generic values use a value-type constraint. |
| 57 | `required` | Helps avoid uninitialized non-nullable members. |
| 58 | `init` | Pairs well with `required` for immutable initialization. |
| 59 | `Array.Empty<string>()` | Returns a non-null empty array. |
| 60 | `return null;` | Should only appear when the method's return type allows null. |
