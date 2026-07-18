# 11 — Exceptions & Resources

How C# handles errors, and how it manages resources like files, database connections, and network sockets. Getting exception handling *right* (not just wrapping everything in try/catch) is a clear junior-vs-mid-level differentiator.

## What an exception is

When something goes wrong, code **throws** an exception — an object describing the error. It propagates up the call stack until something **catches** it, or it crashes the program. Exceptions are for *exceptional* conditions, not normal control flow.

```csharp
int Divide(int a, int b)
{
    if (b == 0)
        throw new DivideByZeroException("Cannot divide by zero");
    return a / b;
}
```

## try / catch / finally

```csharp
try
{
    string content = File.ReadAllText("data.txt");
    int value = int.Parse(content);
    Console.WriteLine(value);
}
catch (FileNotFoundException ex)
{
    Console.WriteLine($"File missing: {ex.Message}");
}
catch (FormatException ex)
{
    Console.WriteLine($"Bad content: {ex.Message}");
}
catch (Exception ex)            // catch-all — put LAST, most general goes last
{
    Console.WriteLine($"Unexpected: {ex.Message}");
}
finally
{
    // Runs no matter what — exception or not, even on return.
    // Use for cleanup. (Often replaced by 'using', see below.)
    Console.WriteLine("Done");
}
```

Rules:
- Catch **specific** exception types first, general (`Exception`) last. The first matching `catch` wins.
- `finally` always runs — use it for cleanup that must happen regardless.
- Don't catch what you can't handle. An empty `catch { }` that swallows errors silently is one of the worst habits in the field.

### The exception object

```csharp
catch (Exception ex)
{
    ex.Message;        // human-readable description
    ex.StackTrace;     // where it happened (the call chain)
    ex.InnerException; // the underlying exception, if this one wraps another
    ex.GetType();      // the exception's concrete type
}
```

## Common built-in exceptions

| Exception | Thrown when |
|---|---|
| `ArgumentNullException` | A required argument was null |
| `ArgumentException` | An argument is invalid |
| `ArgumentOutOfRangeException` | A value is outside the allowed range |
| `InvalidOperationException` | The object is in a wrong state for this call |
| `NullReferenceException` | You dereferenced null (usually a bug, not to catch) |
| `KeyNotFoundException` | Dictionary key missing |
| `FormatException` | Parsing failed |
| `FileNotFoundException`, `IOException` | File/IO problems |
| `NotImplementedException` | Stub not done yet |
| `TimeoutException`, `HttpRequestException` | Network/HTTP issues |

## Throwing exceptions and validating arguments

Validate inputs at the top of methods (guard clauses) and throw meaningful exceptions:

```csharp
public void Transfer(Account from, Account to, decimal amount)
{
    ArgumentNullException.ThrowIfNull(from);          // modern helpers
    ArgumentNullException.ThrowIfNull(to);
    ArgumentOutOfRangeException.ThrowIfNegativeOrZero(amount);

    if (from.Balance < amount)
        throw new InvalidOperationException("Insufficient funds");

    from.Balance -= amount;
    to.Balance += amount;
}
```

The `ThrowIfNull` / `ThrowIf...` static helpers (modern .NET) are the clean way to guard. They include the parameter name automatically.

### Re-throwing correctly

If you catch, do something, and want the exception to continue:

```csharp
catch (Exception ex)
{
    _logger.LogError(ex, "Failed to process order {Id}", order.Id);
    throw;          // RE-THROW — preserves the original stack trace
    // throw ex;    // WRONG — resets the stack trace, hides where it really came from
}
```

**Use bare `throw;`**, never `throw ex;`. This is a classic interview gotcha and a real bug source.

### Wrapping exceptions

Sometimes you catch a low-level exception and throw a more meaningful one, preserving the original as `InnerException`:

```csharp
catch (SqlException ex)
{
    throw new DataAccessException("Failed to load customer", ex);
}
```

## Custom exceptions

Define your own for domain-specific errors. Inherit from `Exception` and provide the standard constructors:

```csharp
public class InsufficientFundsException : Exception
{
    public decimal Shortfall { get; }

    public InsufficientFundsException(decimal shortfall)
        : base($"Short by {shortfall:C}")
    {
        Shortfall = shortfall;
    }
}

// throw new InsufficientFundsException(25.50m);
```

Add custom exceptions when callers need to handle a specific failure differently. Don't create one per error message — only when the *type* matters for handling.

## Exception filters (`when`)

Catch only when an extra condition holds:

```csharp
try { /* ... */ }
catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
{
    // handle 404 specifically
}
catch (HttpRequestException ex) when (ex.StatusCode >= HttpStatusCode.InternalServerError)
{
    // handle 5xx
}
```

## Exceptions vs the `Try` pattern — when to use which

Throwing is relatively expensive and is for *exceptional* situations. For expected, routine failures (parsing user input, checking a key), prefer the non-throwing `Try` pattern:

```csharp
// Expected to sometimes fail (bad user input) -> Try pattern, no exception:
if (int.TryParse(input, out int n)) { /* use n */ }
if (dict.TryGetValue(key, out var value)) { /* use value */ }

// Genuinely exceptional (file should exist, DB should be up) -> let it throw,
// catch at a sensible boundary.
```

Guideline: **don't use exceptions for normal control flow.** If "the thing didn't match" happens routinely, model it with a return value (bool, nullable, `Result<T>`), not an exception.

## Where to handle exceptions

Don't wrap every line in try/catch. Handle at **boundaries** where you can actually do something useful:
- In a web API, a global exception-handling middleware turns unhandled exceptions into proper HTTP error responses (Chapter 20). Most of your business code just lets exceptions bubble up.
- Catch locally only when you can recover (retry, fall back to a default, add context and rethrow).
- **Always log** exceptions you catch (Chapter 18 covers logging).

```csharp
// Typical service method — no try/catch; let failures bubble to the boundary.
public async Task<Order> PlaceOrder(OrderRequest request)
{
    var customer = await _customers.GetAsync(request.CustomerId)
        ?? throw new NotFoundException($"Customer {request.CustomerId} not found");
    // ...normal happy-path logic...
    return order;
}
```

## Resource management: `IDisposable` and `using`

Files, database connections, network sockets, and streams hold **unmanaged resources** that the garbage collector won't promptly release. Types holding such resources implement `IDisposable`, which has a `Dispose()` method to release them. You must call `Dispose()` — and the `using` statement does it for you, even if an exception is thrown.

```csharp
// using statement — Dispose() is called automatically at the end of the block:
using (var reader = new StreamReader("data.txt"))
{
    string? line = reader.ReadLine();
    // ...
}   // reader.Dispose() runs here, even if an exception occurred

// using DECLARATION (modern, less nesting) — disposed at end of the enclosing scope:
void Process()
{
    using var reader = new StreamReader("data.txt");
    string content = reader.ReadToEnd();
    // reader.Dispose() runs when Process() returns
}
```

**Always wrap disposable resources in `using`.** Forgetting to dispose leaks file handles, connections, and memory — a common production bug. Things you'll commonly `using`: `StreamReader`/`StreamWriter`, `FileStream`, `HttpClient` responses, `DbConnection`, `DbContext` (sometimes, via DI), `MemoryStream`.

### Writing your own `IDisposable`

If your class owns disposable resources, implement `IDisposable` and dispose them:

```csharp
public class FileLogger : IDisposable
{
    private readonly StreamWriter _writer;

    public FileLogger(string path) => _writer = new StreamWriter(path, append: true);

    public void Log(string message) => _writer.WriteLine($"{DateTime.UtcNow:o} {message}");

    public void Dispose() => _writer.Dispose();   // release what we own
}

using var logger = new FileLogger("app.log");
logger.Log("started");
```

(The full "dispose pattern" with a `protected virtual Dispose(bool)` and finalizers exists for classes holding *unmanaged* handles directly — you rarely need it. Disposing managed `IDisposable` fields as above covers almost all real cases.)

### `IAsyncDisposable` / `await using`

Async resources (some streams, DB connections) implement `IAsyncDisposable`; dispose them with `await using`:

```csharp
await using var connection = new SqlConnection(connectionString);
```

We'll see this naturally once we cover async (Chapter 13).

## Exercises

1. Write a `SafeDivide(int a, int b)` that returns a `Result<int>` (from Chapter 9) instead of throwing on divide-by-zero.
2. Write a method that reads a file and parses each line as an int, catching `FormatException` per line and reporting which lines were bad (don't let one bad line abort the whole thing).
3. Create a custom `ValidationException` carrying a list of error messages; throw it from a `Validate(user)` method and catch it to print all errors.
4. Implement an `IDisposable` `TempFile` class that creates a temp file in its constructor and deletes it in `Dispose`. Use it with `using` and confirm the file is gone afterward.

---

**Next:** [12 — Delegates, Events & Lambdas](./12-delegates-events-lambdas.md)
