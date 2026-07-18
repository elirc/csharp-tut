# 13 — Async & Await

Asynchronous programming is **non-negotiable** for modern .NET backend work. Every database call, HTTP request, and file read in a web app should be async. This chapter explains *why*, the `Task`/`async`/`await` model, and the pitfalls that cause real production incidents. Read it twice.

## The problem async solves

A web server handles many requests at once using a limited pool of threads. When a request calls the database, that operation takes (say) 50ms during which **nothing happens on that thread** — it's just waiting for the network/disk. If the thread *blocks* (sits idle) waiting, it can't serve other requests. Under load you run out of threads and the whole app stalls, even though the CPU is idle.

**Async** lets a thread *release* itself while waiting for I/O and go serve other requests. When the I/O completes, the work resumes (possibly on a different thread). Same hardware, vastly more throughput. That's the entire point: **async is about scalability under I/O, not about doing things faster in isolation.**

## `Task` and `Task<T>`

A **`Task`** represents an operation in progress that will complete in the future (like a "promise" elsewhere).
- `Task` — an async operation returning nothing.
- `Task<T>` — an async operation that will produce a `T`.

```csharp
Task<string> downloadTask = httpClient.GetStringAsync(url);  // starts; returns immediately
// ...the download runs; we have a handle to it...
string result = await downloadTask;                          // wait for the value
```

## `async` and `await`

- Mark a method **`async`** to use `await` inside it.
- **`await`** unwraps a `Task<T>` to its `T`, *asynchronously* — it pauses the method and frees the thread until the task completes, then resumes.

```csharp
public async Task<string> GetUserNameAsync(int id)
{
    Customer customer = await _repository.GetByIdAsync(id);   // await an async call
    return customer.Name;
}
```

Reading it: "start `GetByIdAsync`; while it runs, release this thread; when it's done, assign the result to `customer` and continue." The method *looks* synchronous but doesn't block.

### Return types for async methods

- **`Task<T>`** — async method returning a value. Most common.
- **`Task`** — async method returning nothing (the async version of `void`).
- **`void`** — **avoid.** `async void` can't be awaited and its exceptions crash the process. The *only* valid use is event handlers. Never write `async void` for normal methods.
- **`ValueTask<T>`** — a performance optimization for hot paths that often complete synchronously; use only when profiling says so.

```csharp
public async Task SaveAsync(Order order)          // returns Task (no value)
{
    await _db.SaveChangesAsync();
}

public async Task<int> CountAsync()               // returns Task<int>
{
    return await _db.Orders.CountAsync();
}
```

### The naming convention

Async methods end with **`Async`** by convention: `GetByIdAsync`, `SaveChangesAsync`, `ReadAllTextAsync`. Follow it — teams and tooling rely on it.

## Async all the way

Once you `await`, the asynchrony flows up the call chain. The caller must also be `async` and `await` you, and so on, up to the entry point (in ASP.NET, the framework awaits your controller actions; in a console app, `Main` can be `async Task`).

```csharp
// Console app entry point can be async:
static async Task Main(string[] args)
{
    var data = await FetchDataAsync();
    Console.WriteLine(data);
}
```

**Don't "break the chain"** by blocking on a task (see deadlocks below). If a method calls async code, make it async too.

## Running things concurrently with `Task.WhenAll`

`await`ing tasks one after another runs them **sequentially**. To run independent async operations **concurrently**, start them all, then await together:

```csharp
// Sequential — total time ≈ sum of both (slow):
var a = await GetUserAsync(1);
var b = await GetUserAsync(2);

// Concurrent — total time ≈ the slower of the two (fast):
Task<User> taskA = GetUserAsync(1);    // start (don't await yet)
Task<User> taskB = GetUserAsync(2);    // start
await Task.WhenAll(taskA, taskB);      // wait for both
User userA = taskA.Result;             // safe NOW (already completed)
User userB = taskB.Result;

// Or with results directly:
User[] users = await Task.WhenAll(
    ids.Select(id => GetUserAsync(id)));   // fire off N calls, await all
```

`Task.WhenAll` is how you parallelize independent I/O. `Task.WhenAny` completes when the first task finishes (useful for timeouts/races).

> Be careful running many DB calls concurrently against a *single* `DbContext` — EF Core contexts are **not** thread-safe (Chapter 19). `WhenAll` is for independent operations (separate HTTP calls, separate contexts).

## Exceptions in async code

Exceptions thrown in an async method are captured in the returned `Task` and re-thrown when you `await` it. So you catch them normally:

```csharp
try
{
    var data = await FetchDataAsync();
}
catch (HttpRequestException ex)
{
    _logger.LogError(ex, "Fetch failed");
}
```

(With `Task.WhenAll`, if multiple tasks throw, `await` surfaces the first exception; the `Task.Exception` holds all of them as an `AggregateException`.)

## CancellationToken — cooperative cancellation

Long-running and I/O operations should accept a `CancellationToken` so callers can cancel (request timeout, user navigates away, app shutting down). You pass it down the chain:

```csharp
public async Task<List<Order>> GetOrdersAsync(int customerId, CancellationToken ct)
{
    return await _db.Orders
        .Where(o => o.CustomerId == customerId)
        .ToListAsync(ct);          // pass the token to the framework call
}
```

In ASP.NET Core, controller actions can take a `CancellationToken` parameter and the framework supplies one tied to the request lifetime. Honor it by passing it to every async call you make. To create one with a timeout:

```csharp
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
var result = await SomeOperationAsync(cts.Token);   // cancels after 5s
```

Cancellation surfaces as an `OperationCanceledException` / `TaskCanceledException` when awaited.

## The deadlock trap (the #1 async bug)

**Never block on async code with `.Result` or `.Wait()`.** In certain contexts (older ASP.NET, UI apps) this causes a **deadlock**: the blocked thread is the very one the continuation needs to resume on, so nothing proceeds.

```csharp
// 💥 DON'T — can deadlock, and even when it doesn't, it wastes a thread:
var data = FetchDataAsync().Result;
SomeAsyncMethod().Wait();

// ✅ DO — await it (make your method async):
var data = await FetchDataAsync();
```

If you remember one rule from this chapter: **`async` all the way down; never `.Result`/`.Wait()`.** The only place you might see `.Result` safely is on a task you *know* is already completed (e.g., right after `Task.WhenAll`).

### `ConfigureAwait(false)` (library code)

In **library** code (not ASP.NET app code), you'll see `await SomethingAsync().ConfigureAwait(false)`. It tells the runtime "don't bother resuming on the original context," which avoids some deadlock scenarios and is slightly faster. In ASP.NET Core app code it's unnecessary (there's no special synchronization context). Rule: add `ConfigureAwait(false)` in reusable libraries; don't bother in app/controller code. Don't stress about it early.

## CPU-bound work: `Task.Run`

`async`/`await` is for **I/O-bound** waiting. For genuinely **CPU-bound** work (heavy computation) that you want off the current thread, use `Task.Run`:

```csharp
var result = await Task.Run(() => ExpensiveCalculation(data));
```

But don't wrap I/O calls in `Task.Run` — that just wastes a thread. Use the natively-async I/O method instead (`ReadAllTextAsync`, not `Task.Run(() => ReadAllText(...))`). In a web app, `Task.Run` is rarely the right tool; it's more for desktop apps keeping the UI responsive.

## A complete realistic example

```csharp
public class WeatherService
{
    private readonly HttpClient _http;
    private readonly ILogger<WeatherService> _logger;

    public WeatherService(HttpClient http, ILogger<WeatherService> logger)
    {
        _http = http;
        _logger = logger;
    }

    public async Task<Forecast?> GetForecastAsync(string city, CancellationToken ct = default)
    {
        try
        {
            // Async HTTP call — thread is freed while waiting:
            var response = await _http.GetAsync($"/weather?city={city}", ct);
            response.EnsureSuccessStatusCode();

            // Async deserialization:
            var forecast = await response.Content
                .ReadFromJsonAsync<Forecast>(cancellationToken: ct);
            return forecast;
        }
        catch (HttpRequestException ex)
        {
            _logger.LogError(ex, "Weather lookup failed for {City}", city);
            return null;
        }
    }

    // Fetch several cities concurrently:
    public async Task<List<Forecast>> GetManyAsync(IEnumerable<string> cities, CancellationToken ct = default)
    {
        var tasks = cities.Select(c => GetForecastAsync(c, ct));
        var results = await Task.WhenAll(tasks);
        return results.Where(f => f is not null).Select(f => f!).ToList();
    }
}
```

## Mental checklist for writing async

1. Does this method do I/O (DB, HTTP, file)? → Make it `async`, return `Task`/`Task<T>`, name it `...Async`.
2. Am I calling an async method? → `await` it. Never `.Result`/`.Wait()`.
3. Are several awaits independent? → Start them and `Task.WhenAll` to run concurrently.
4. Does it support cancellation? → Accept and pass a `CancellationToken`.
5. Library code? → `ConfigureAwait(false)`. App/controller code? → don't bother.
6. Never write `async void` (except event handlers).

## Exercises

1. Convert a synchronous file-reading method to async using `File.ReadAllTextAsync`.
2. Write `Task<int> SumPagesAsync(string[] urls)` that downloads each URL's length and sums them — first sequentially, then rewritten with `Task.WhenAll`. Measure both with a `Stopwatch`.
3. Add a `CancellationToken` to an async loop that processes items, checking `ct.ThrowIfCancellationRequested()` each iteration, and cancel it after 1 second.
4. Explain in a comment why `var x = GetDataAsync().Result;` is dangerous and what to write instead.

---

**Next:** [14 — Pattern Matching & Modern Syntax](./14-pattern-matching-and-modern.md)
