# 11 - Exceptions & Resources Examples

These snippets reinforce throwing, catching, filters, custom exceptions, `using`, disposal, and async disposal.

## Example 1 - Throwing an exception

```csharp
throw new InvalidOperationException("The operation cannot continue.");
```

Throw an exception when code cannot complete its contract.

## Example 2 - Basic try catch

```csharp
try
{
    int value = int.Parse("abc");
}
catch (FormatException)
{
    Console.WriteLine("Not a valid number.");
}
```

Catch the specific exception type you know how to handle.

## Example 3 - Finally block

```csharp
try
{
    Console.WriteLine("Work");
}
finally
{
    Console.WriteLine("Cleanup");
}
```

`finally` runs whether the try block succeeds or throws.

## Example 4 - Exception object

```csharp
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

The exception object contains details such as the message and stack trace.

## Example 5 - Argument validation

```csharp
static void SetAge(int age)
{
    if (age < 0) throw new ArgumentOutOfRangeException(nameof(age));
}
```

Use argument exceptions when callers pass invalid method inputs.

## Example 6 - Null validation

```csharp
static void Print(string text)
{
    ArgumentNullException.ThrowIfNull(text);
    Console.WriteLine(text);
}
```

`ThrowIfNull` is the modern compact way to validate non-null arguments.

## Example 7 - Correct rethrow

```csharp
try
{
    Dangerous();
}
catch
{
    Console.WriteLine("Logging");
    throw;
}
```

`throw;` preserves the original stack trace.

## Example 8 - Wrapping an exception

```csharp
try
{
    LoadSettings();
}
catch (IOException ex)
{
    throw new InvalidOperationException("Could not load settings.", ex);
}
```

Wrapping adds context while preserving the original exception as `InnerException`.

## Example 9 - Custom exception

```csharp
public class PaymentFailedException : Exception
{
    public PaymentFailedException(string message) : base(message) { }
}
```

Custom exceptions are useful when callers need to catch a domain-specific failure.

## Example 10 - Exception filter

```csharp
catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
{
    Console.WriteLine("Resource missing.");
}
```

`when` filters catch only exceptions that meet an extra condition.

## Example 11 - Try pattern

```csharp
if (int.TryParse("42", out int number))
{
    Console.WriteLine(number);
}
```

Use `Try` methods for expected failures such as invalid user input.

## Example 12 - Let unexpected exceptions bubble

```csharp
static decimal Divide(decimal a, decimal b)
{
    return a / b;
}
```

Do not catch exceptions just to hide them; callers may be better positioned to handle failure.

## Example 13 - using declaration

```csharp
using var reader = File.OpenText("notes.txt");
string? firstLine = reader.ReadLine();
```

A `using` declaration disposes the resource at the end of the current scope.

## Example 14 - using statement

```csharp
using (var stream = File.OpenRead("notes.txt"))
{
    Console.WriteLine(stream.Length);
}
```

A `using` statement disposes the resource when the block exits.

## Example 15 - Implement IDisposable

```csharp
public class TempHandle : IDisposable
{
    public void Dispose() => Console.WriteLine("Released");
}
```

Implement `IDisposable` when a type owns resources that must be released.

## Example 16 - Dispose pattern for fields

```csharp
public sealed class LogFile : IDisposable
{
    private readonly StreamWriter _writer = File.CreateText("log.txt");
    public void Dispose() => _writer.Dispose();
}
```

Disposing owned fields prevents file handles and similar resources from staying open.

## Example 17 - await using

```csharp
await using var stream = File.Create("data.bin");
await stream.WriteAsync(new byte[] { 1, 2, 3 });
```

`await using` supports resources that clean up asynchronously.

## Example 18 - Avoid catching everything too early

```csharp
try
{
    ProcessOrder();
}
catch (PaymentFailedException ex)
{
    Console.WriteLine(ex.Message);
}
```

Handle the exception type you can respond to, and let unrelated failures surface.

## Example 19 - Fail fast

```csharp
static void Save(string path)
{
    if (string.IsNullOrWhiteSpace(path)) throw new ArgumentException("Path is required.", nameof(path));
}
```

Failing early makes bugs easier to locate and prevents invalid state from spreading.

## Example 20 - Cleanup without catch

```csharp
using var connection = OpenConnection();
RunQuery(connection);
```

You can ensure cleanup with `using` even when you do not catch exceptions locally.

Helper methods used above:

```csharp
static void Dangerous() { }
static void LoadSettings() { }
static void ProcessOrder() { }
static IDisposable OpenConnection() => new TempHandle();
static void RunQuery(IDisposable connection) { }
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `throw new ArgumentException("Name is required", nameof(name));` | Reports invalid caller input with the parameter name. |
| 22 | `throw new ArgumentNullException(nameof(customer));` | Reports a required reference that was null. |
| 23 | `throw new ArgumentOutOfRangeException(nameof(age));` | Reports a value outside the accepted range. |
| 24 | `throw new NotSupportedException();` | Signals that an operation is intentionally unsupported. |
| 25 | `throw new NotImplementedException();` | Marks code that has not been written yet. |
| 26 | `catch (IOException ex)` | Handles file and stream failures specifically. |
| 27 | `catch (UnauthorizedAccessException)` | Handles missing file-system permissions. |
| 28 | `catch (TimeoutException)` | Handles operations that exceeded their time limit. |
| 29 | `catch (Exception ex) when (ex.Message.Contains("retry"))` | Uses a filter to catch only some exceptions. |
| 30 | `catch { throw; }` | Rethrows while preserving the original stack trace. |
| 31 | `catch (Exception ex) { throw new AppException("Save failed", ex); }` | Wraps a low-level error with higher-level context. |
| 32 | `finally { cleanup.Dispose(); }` | Ensures cleanup runs after success or failure. |
| 33 | `using var file = File.OpenRead(path);` | Disposes the stream when the current scope ends. |
| 34 | `using (var file = File.OpenRead(path)) { }` | Limits the resource lifetime to a block. |
| 35 | `await using var resource = CreateAsyncResource();` | Uses async disposal for resources that support it. |
| 36 | `public sealed class Worker : IDisposable` | A disposable class often should be sealed unless designed for inheritance. |
| 37 | `private bool _disposed;` | Tracks whether cleanup already happened. |
| 38 | `GC.SuppressFinalize(this);` | Tells the runtime finalization is unnecessary after manual disposal. |
| 39 | `ObjectDisposedException.ThrowIf(_disposed, this);` | Throws consistently when a disposed object is used. |
| 40 | `bool ok = decimal.TryParse(text, out var amount);` | Try methods handle expected invalid input without exceptions. |
| 41 | `if (!File.Exists(path)) return;` | Sometimes a branch is clearer than catching a predictable exception. |
| 42 | `try { } catch { }` | Empty catches hide bugs and should almost always be avoided. |
| 43 | `catch (Exception ex) { logger.LogError(ex, "Failed"); throw; }` | Log and rethrow when the caller still needs to see the failure. |
| 44 | `throw;` | Use this inside catch to preserve original error location. |
| 45 | `throw ex;` | Avoid this because it resets useful stack trace information. |
| 46 | `ExceptionDispatchInfo.Capture(ex).Throw();` | Rethrows later while preserving exception details. |
| 47 | `public class DomainException : Exception` | Defines a base exception for domain-specific failures. |
| 48 | `public PaymentFailedException() { }` | Custom exceptions can expose standard constructors. |
| 49 | `public PaymentFailedException(string message) : base(message) { }` | Allows callers to provide a useful message. |
| 50 | `public PaymentFailedException(string message, Exception inner) : base(message, inner) { }` | Supports exception wrapping in custom exceptions. |
| 51 | `TaskCanceledException` | Often appears when async work is canceled or times out. |
| 52 | `OperationCanceledException` | The standard exception for cooperative cancellation. |
| 53 | `catch (OperationCanceledException) when (token.IsCancellationRequested)` | Distinguishes expected cancellation from other failures. |
| 54 | `File.ReadAllText(path)` | Can throw exceptions for missing files, permissions, or invalid paths. |
| 55 | `Directory.CreateDirectory(path)` | Usually succeeds even if the directory already exists. |
| 56 | `response.EnsureSuccessStatusCode()` | Converts unsuccessful HTTP responses into exceptions. |
| 57 | `try { await work; } catch (Exception ex)` | Async exceptions are caught when the task is awaited. |
| 58 | `using var cts = new CancellationTokenSource();` | Cancellation sources are disposable resources. |
| 59 | `token.ThrowIfCancellationRequested();` | Cooperatively stops work when cancellation is requested. |
| 60 | `return Result.Failure(error);` | Result objects can represent expected failures without exceptions. |
