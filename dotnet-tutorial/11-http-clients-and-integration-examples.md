# 11 - HTTP Clients and Integration Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `HttpClient` | Sends HTTP requests. |
| 2 | `IHttpClientFactory` | Creates managed HttpClient instances. |
| 3 | `AddHttpClient()` | Registers factory support. |
| 4 | `AddHttpClient<TClient>()` | Registers typed client. |
| 5 | `BaseAddress` | Common API root URL. |
| 6 | `GetAsync()` | Sends GET request. |
| 7 | `PostAsJsonAsync()` | Sends JSON POST request. |
| 8 | `ReadFromJsonAsync<T>()` | Reads JSON response. |
| 9 | `GetFromJsonAsync<T>()` | GET plus JSON deserialize. |
| 10 | `EnsureSuccessStatusCode()` | Throws on non-success status. |
| 11 | `IsSuccessStatusCode` | Checks success manually. |
| 12 | `StatusCode` | Reads HTTP status. |
| 13 | `Authorization header` | Sends auth credentials. |
| 14 | `User-Agent header` | Identifies client app. |
| 15 | `CancellationToken` | Cancels HTTP work. |
| 16 | `Timeout` | Limits request duration. |
| 17 | `HttpRequestMessage` | Full custom request object. |
| 18 | `DelegatingHandler` | Custom outgoing request pipeline. |
| 19 | `resilience policy` | Retry/timeout/circuit-breaker behavior. |
| 20 | `typed client` | Keeps integration logic focused. |
