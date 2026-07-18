# 17 — HTTP & Calling APIs

Most backend services *consume* other services over HTTP. C#'s tool for this is `HttpClient`. There are some important do's and don'ts (especially around lifetime) that trip up beginners and cause production issues. This chapter gets you calling APIs correctly.

## The golden rule of `HttpClient`

**Do not `new HttpClient()` per request and dispose it.** Despite implementing `IDisposable`, creating a fresh `HttpClient` for every call exhausts OS network sockets under load (sockets linger in `TIME_WAIT`), causing `SocketException`s. This is *the* classic `HttpClient` bug.

The correct approaches, in order of preference:
1. **`IHttpClientFactory`** (in any DI-based app — ASP.NET, worker services). This is the right answer in real apps.
2. A single, **long-lived shared `HttpClient`** for the app's lifetime (fine for small console apps/scripts).

We'll show both, but in real CRUD services you'll use the factory (Chapter 18 covers DI).

## Quick start (shared instance, console app)

```csharp
using System.Net.Http;
using System.Net.Http.Json;

// One shared instance for the whole program:
var http = new HttpClient { BaseAddress = new Uri("https://api.example.com/") };

// GET and deserialize JSON in one call (the JSON extension methods are gold):
var product = await http.GetFromJsonAsync<Product>("products/42");
Console.WriteLine(product?.Name);

var products = await http.GetFromJsonAsync<List<Product>>("products");
```

`GetFromJsonAsync<T>` (from `System.Net.Http.Json`) does the GET, checks success, and deserializes — the everyday workhorse. It uses camelCase-friendly options by default.

## The verbs

```csharp
// GET raw string / bytes:
string body = await http.GetStringAsync("status");
byte[] data = await http.GetByteArrayAsync("image.png");

// GET typed:
var item = await http.GetFromJsonAsync<Item>("items/1");

// POST a body as JSON, get the response deserialized:
var newItem = new CreateItem("Widget", 9.99m);
HttpResponseMessage resp = await http.PostAsJsonAsync("items", newItem);
resp.EnsureSuccessStatusCode();
var created = await resp.Content.ReadFromJsonAsync<Item>();

// PUT (full update):
await http.PutAsJsonAsync($"items/{id}", updatedItem);

// DELETE:
await http.DeleteAsync($"items/{id}");

// PATCH (partial) — needs a bit more setup:
var patch = new HttpRequestMessage(HttpMethod.Patch, $"items/{id}")
{
    Content = JsonContent.Create(new { price = 12.99m })
};
await http.SendAsync(patch);
```

## Inspecting the response

`HttpResponseMessage` carries status, headers, and content:

```csharp
HttpResponseMessage resp = await http.GetAsync("items/42");

if (resp.IsSuccessStatusCode)              // 2xx
{
    var item = await resp.Content.ReadFromJsonAsync<Item>();
}
else if (resp.StatusCode == HttpStatusCode.NotFound)   // 404
{
    Console.WriteLine("Not found");
}
else
{
    string error = await resp.Content.ReadAsStringAsync();
    Console.WriteLine($"Error {(int)resp.StatusCode}: {error}");
}

// EnsureSuccessStatusCode() throws HttpRequestException on non-2xx — convenient
// when you want failures to bubble up:
resp.EnsureSuccessStatusCode();
```

Decide deliberately: call `EnsureSuccessStatusCode()` when any non-success should throw, or check `StatusCode` when you want to handle specific codes (404, 401, 429) differently.

## Headers, auth, query strings

```csharp
// Default headers on the client (apply to every request):
http.DefaultRequestHeaders.Add("X-Api-Key", "secret");
http.DefaultRequestHeaders.Authorization =
    new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
http.DefaultRequestHeaders.Accept.Add(
    new System.Net.Http.Headers.MediaTypeWithQualityHeaderValue("application/json"));

// Per-request headers via HttpRequestMessage:
var req = new HttpRequestMessage(HttpMethod.Get, "items");
req.Headers.Add("X-Trace-Id", Guid.NewGuid().ToString());
var resp = await http.SendAsync(req);

// Build query strings safely:
var query = new Dictionary<string, string?> { ["page"] = "2", ["size"] = "20" };
var url = QueryHelpers.AddQueryString("items", query);   // from Microsoft.AspNetCore.WebUtilities
```

## `IHttpClientFactory` — the production approach

In a DI app, register named or typed clients in startup, and the factory manages connection pooling and lifetime for you.

### Typed client (recommended)

Wrap the API in a class and let DI inject a configured `HttpClient`:

```csharp
public class WeatherApiClient
{
    private readonly HttpClient _http;
    public WeatherApiClient(HttpClient http) => _http = http;

    public async Task<Forecast?> GetForecastAsync(string city, CancellationToken ct = default)
    {
        return await _http.GetFromJsonAsync<Forecast>($"forecast?city={city}", ct);
    }
}
```

Register it (in `Program.cs` — full DI in Chapter 18):

```csharp
builder.Services.AddHttpClient<WeatherApiClient>(client =>
{
    client.BaseAddress = new Uri("https://api.weather.com/");
    client.Timeout = TimeSpan.FromSeconds(10);
    client.DefaultRequestHeaders.Add("X-Api-Key", apiKey);
});
```

Now anywhere you need it, inject `WeatherApiClient` and call `GetForecastAsync`. The factory handles the socket/lifetime concerns. This is how teams call external APIs.

## Timeouts, cancellation, and resilience

```csharp
http.Timeout = TimeSpan.FromSeconds(30);   // client-wide timeout

// Per-call cancellation/timeout:
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
var resp = await http.GetAsync("slow-endpoint", cts.Token);
```

Real services add **retries with backoff**, **circuit breakers**, and **timeouts per attempt** for resilience. The standard tool is **Polly** (often via `Microsoft.Extensions.Http.Resilience` / `AddStandardResilienceHandler`). You don't need to master it now, but know that "what happens when the downstream API is flaky?" is a real concern and Polly is the answer:

```csharp
builder.Services.AddHttpClient<WeatherApiClient>(/* config */)
       .AddStandardResilienceHandler();    // retries, timeouts, circuit breaker
```

## Error handling pattern

```csharp
public async Task<Forecast?> TryGetForecastAsync(string city, CancellationToken ct = default)
{
    try
    {
        return await _http.GetFromJsonAsync<Forecast>($"forecast?city={city}", ct);
    }
    catch (HttpRequestException ex)
    {
        _logger.LogError(ex, "Weather API call failed for {City}", city);
        return null;
    }
    catch (TaskCanceledException) when (!ct.IsCancellationRequested)
    {
        _logger.LogWarning("Weather API timed out for {City}", city);
        return null;
    }
}
```

Note the distinction: a `TaskCanceledException` can be a genuine cancellation (the caller's token fired) *or* a timeout — the `when` filter separates them.

## Mental model summary

- Use `GetFromJsonAsync<T>` / `PostAsJsonAsync` for the common JSON cases.
- **Never** new-up-and-dispose `HttpClient` per request. Use `IHttpClientFactory`/typed clients in apps; one shared instance in scripts.
- Always set a timeout and pass a `CancellationToken`.
- Decide between `EnsureSuccessStatusCode()` (throw) and explicit status handling.
- For production resilience, reach for Polly / standard resilience handler.

## Exercises

1. Call a public JSON API (e.g. `https://api.github.com/users/dotnet` — set a `User-Agent` header, GitHub requires it) and deserialize the response into a record with a few fields.
2. Write a typed client class wrapping that API with a `GetUserAsync(string login)` method.
3. POST a JSON object to `https://httpbin.org/post` and read back the echoed response.
4. Add a 2-second timeout via `CancellationTokenSource` and handle the timeout gracefully, distinguishing it from a real cancellation.

---

**Next:** [18 — Dependency Injection & Configuration](./18-dependency-injection-and-config.md)
