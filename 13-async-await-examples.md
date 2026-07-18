# 13 - Async & Await Examples

These snippets reinforce `Task`, `Task<T>`, `async`, `await`, concurrency, exceptions, cancellation, and CPU-bound work.

## Example 1 - Returning Task

```csharp
static async Task PauseAsync()
{
    await Task.Delay(500);
}
```

Use `Task` for async methods that do not return a result.

## Example 2 - Returning Task<T>

```csharp
static async Task<int> GetCountAsync()
{
    await Task.Delay(100);
    return 42;
}
```

Use `Task<T>` when an async method returns a value.

## Example 3 - Awaiting a task

```csharp
int count = await GetCountAsync();
Console.WriteLine(count);
```

`await` pauses the current async method until the task completes.

## Example 4 - Async naming

```csharp
static Task<string> LoadNameAsync()
{
    return Task.FromResult("Ada");
}
```

The `Async` suffix signals that a method returns a task and should usually be awaited.

## Example 5 - Async all the way

```csharp
static async Task PrintNameAsync()
{
    string name = await LoadNameAsync();
    Console.WriteLine(name);
}
```

Async code composes best when callers also become async.

## Example 6 - Starting tasks

```csharp
Task<int> a = GetCountAsync();
Task<int> b = GetCountAsync();
int[] results = await Task.WhenAll(a, b);
```

Starting tasks before awaiting them allows independent work to run concurrently.

## Example 7 - Sequential awaits

```csharp
int first = await GetCountAsync();
int second = await GetCountAsync();
```

Sequential awaits are simpler but wait for each operation before starting the next.

## Example 8 - Handling async exceptions

```csharp
try
{
    await FailsAsync();
}
catch (InvalidOperationException ex)
{
    Console.WriteLine(ex.Message);
}
```

Exceptions from awaited tasks are observed at the `await`.

## Example 9 - Task.WhenAll exception

```csharp
try
{
    await Task.WhenAll(FailsAsync(), FailsAsync());
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

`WhenAll` completes as failed if any of its tasks fail.

## Example 10 - CancellationToken parameter

```csharp
static async Task WorkAsync(CancellationToken cancellationToken)
{
    await Task.Delay(1000, cancellationToken);
}
```

Pass cancellation tokens into async APIs so callers can request cancellation.

## Example 11 - Creating a cancellation source

```csharp
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(2));
await WorkAsync(cts.Token);
```

`CancellationTokenSource` creates and controls a cancellation token.

## Example 12 - Checking cancellation manually

```csharp
static async Task LoopAsync(CancellationToken token)
{
    while (true)
    {
        token.ThrowIfCancellationRequested();
        await Task.Delay(100, token);
    }
}
```

Long-running loops should cooperate with cancellation explicitly.

## Example 13 - Avoid blocking on Task

```csharp
// Avoid this in async-capable code:
// string name = LoadNameAsync().Result;
```

Blocking on tasks can cause deadlocks and wastes threads.

## Example 14 - ConfigureAwait in library code

```csharp
string name = await LoadNameAsync().ConfigureAwait(false);
```

Library code can use `ConfigureAwait(false)` when it does not need to resume on the captured context.

## Example 15 - CPU-bound work

```csharp
int result = await Task.Run(() => ExpensiveCalculation());
```

`Task.Run` moves CPU-bound work to the thread pool; it is not needed for naturally async I/O.

## Example 16 - ValueTask awareness

```csharp
static ValueTask<int> CachedCountAsync()
{
    return ValueTask.FromResult(10);
}
```

`ValueTask<T>` can reduce allocations in specialized hot paths, but `Task<T>` is the default.

## Example 17 - Async stream

```csharp
static async IAsyncEnumerable<int> CountAsync()
{
    for (int i = 1; i <= 3; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

Async streams produce values over time instead of returning one final result.

## Example 18 - Await foreach

```csharp
await foreach (int value in CountAsync())
{
    Console.WriteLine(value);
}
```

`await foreach` consumes an `IAsyncEnumerable<T>`.

## Example 19 - Async using

```csharp
await using var stream = File.Create("output.bin");
await stream.WriteAsync(new byte[] { 1, 2, 3 });
```

`await using` disposes resources that support asynchronous cleanup.

## Example 20 - Timeout pattern

```csharp
using var timeout = new CancellationTokenSource(TimeSpan.FromSeconds(5));
await WorkAsync(timeout.Token);
```

Timeouts are usually implemented by cancellation rather than by abandoning running work.

Helper methods used above:

```csharp
static async Task FailsAsync()
{
    await Task.Delay(10);
    throw new InvalidOperationException("Failed.");
}

static int ExpensiveCalculation() => Enumerable.Range(1, 1_000_000).Sum();
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `Task completed = Task.CompletedTask;` | Represents already-finished async work with no result. |
| 22 | `Task<int> answer = Task.FromResult(42);` | Creates an already-completed task with a result. |
| 23 | `await Task.Delay(TimeSpan.FromSeconds(1));` | Simulates or waits for asynchronous time passing. |
| 24 | `async Task MainAsync()` | Async methods usually return `Task`. |
| 25 | `async Task<string> LoadAsync()` | Async methods returning values use `Task<T>`. |
| 26 | `var task = LoadAsync();` | Starts async work when the method is called. |
| 27 | `string value = await task;` | Awaits the result from previously started work. |
| 28 | `await using var stream = File.Create(path);` | Disposes async-compatible resources after awaited work. |
| 29 | `await foreach (var item in source)` | Iterates asynchronous streams. |
| 30 | `Task.WhenAll(tasks)` | Waits for many tasks to complete. |
| 31 | `Task.WhenAny(tasks)` | Waits for the first task to complete. |
| 32 | `await Task.WhenAll(loadUsers, loadOrders);` | Runs independent operations concurrently. |
| 33 | `var results = await Task.WhenAll(tasks);` | Collects results from multiple `Task<T>` values. |
| 34 | `using var cts = new CancellationTokenSource();` | Creates a source that can request cancellation. |
| 35 | `cts.Cancel();` | Signals cancellation to token-aware operations. |
| 36 | `new CancellationTokenSource(TimeSpan.FromSeconds(10))` | Creates a token source that cancels after a timeout. |
| 37 | `await client.GetAsync(url, token);` | Passes cancellation to an HTTP request. |
| 38 | `await File.ReadAllTextAsync(path, token);` | Passes cancellation to async file reading. |
| 39 | `token.ThrowIfCancellationRequested();` | Stops CPU-side work cooperatively. |
| 40 | `catch (OperationCanceledException)` | Handles expected cancellation. |
| 41 | `catch (HttpRequestException ex)` | Handles async HTTP request failure. |
| 42 | `ConfigureAwait(false)` | Avoids resuming on a captured context in library code. |
| 43 | `Task.Run(Calculate)` | Moves CPU-bound work to the thread pool. |
| 44 | `await Task.Yield();` | Yields control back to the current scheduler. |
| 45 | `ValueTask<int>` | A lower-allocation alternative for specialized async APIs. |
| 46 | `IAsyncEnumerable<string>` | Represents values that arrive asynchronously over time. |
| 47 | `yield return item;` | Produces a value from an async iterator. |
| 48 | `await foreach (var line in ReadLinesAsync())` | Consumes async stream values one at a time. |
| 49 | `Parallel.ForEach(items, item => Work(item));` | Parallel work is not the same as async I/O. |
| 50 | `SemaphoreSlim gate = new(3);` | Limits concurrency in async workflows. |
| 51 | `await gate.WaitAsync(token);` | Asynchronously waits to enter a concurrency gate. |
| 52 | `gate.Release();` | Releases a semaphore slot after work completes. |
| 53 | `try { await work; } finally { gate.Release(); }` | Ensures concurrency gates are released after failures. |
| 54 | `async void OnClick(object sender, EventArgs e)` | `async void` is mostly reserved for event handlers. |
| 55 | `Task.WaitAll(tasks);` | Blocks threads and is usually avoided in async code. |
| 56 | `task.Result` | Blocks waiting for a result and can cause deadlocks. |
| 57 | `await using var _ = await CreateAsync();` | Awaits creation and disposal of async resources. |
| 58 | `await stream.CopyToAsync(destination, token);` | Copies stream data asynchronously. |
| 59 | `await writer.WriteLineAsync(text);` | Writes text asynchronously. |
| 60 | `return await LoadAsync();` | Awaits before returning, which is useful when local try/catch must observe errors. |
