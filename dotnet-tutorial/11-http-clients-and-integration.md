# 11 - HTTP Clients and Integration

.NET apps often call other HTTP APIs. Use `HttpClient` carefully. In production apps, prefer `IHttpClientFactory`.

Typed client:

```csharp
public class WeatherClient(HttpClient http)
{
    public Task<Forecast?> GetAsync(CancellationToken token)
    {
        return http.GetFromJsonAsync<Forecast>("forecast", token);
    }
}
```

Registration:

```csharp
builder.Services.AddHttpClient<WeatherClient>(client =>
{
    client.BaseAddress = new Uri("https://api.example.com/");
});
```

Real integrations need timeouts, cancellation, retries, error handling, authentication, logging, and tests with mocked responses.

## Exercises

1. Create a typed HTTP client.
2. Register it with `AddHttpClient`.
3. Send a GET request.
4. Handle non-success status codes.
5. Pass a `CancellationToken`.
