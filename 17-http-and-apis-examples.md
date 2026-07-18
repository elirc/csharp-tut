# 17 - HTTP & Calling APIs Examples

These snippets reinforce `HttpClient`, verbs, responses, headers, auth, query strings, typed clients, cancellation, and error handling.

## Example 1 - Shared HttpClient

```csharp
static readonly HttpClient Client = new();
```

Reusing `HttpClient` avoids socket exhaustion caused by creating many short-lived clients.

## Example 2 - GET as string

```csharp
string body = await Client.GetStringAsync("https://example.com");
```

`GetStringAsync` is a quick way to fetch text from a URL.

## Example 3 - GET response object

```csharp
HttpResponseMessage response = await Client.GetAsync("https://example.com");
```

`HttpResponseMessage` lets you inspect status, headers, and content separately.

## Example 4 - Ensure success

```csharp
response.EnsureSuccessStatusCode();
```

This throws an exception for non-success status codes.

## Example 5 - Read response content

```csharp
string json = await response.Content.ReadAsStringAsync();
```

The response content is read asynchronously after the headers arrive.

## Example 6 - POST JSON

```csharp
var payload = new { name = "Ada" };
HttpResponseMessage response = await Client.PostAsJsonAsync("https://api.example.com/users", payload);
```

`PostAsJsonAsync` serializes the object and sends it as a JSON request body.

## Example 7 - PUT JSON

```csharp
await Client.PutAsJsonAsync("https://api.example.com/users/1", payload);
```

PUT commonly replaces or updates a resource at a known URL.

## Example 8 - DELETE

```csharp
HttpResponseMessage response = await Client.DeleteAsync("https://api.example.com/users/1");
```

DELETE requests ask the server to remove a resource.

## Example 9 - Inspect status code

```csharp
if (response.StatusCode == HttpStatusCode.NotFound)
{
    Console.WriteLine("Missing");
}
```

Sometimes you handle specific status codes instead of treating all failures the same.

## Example 10 - Response headers

```csharp
foreach (var header in response.Headers)
{
    Console.WriteLine($"{header.Key}: {string.Join(", ", header.Value)}");
}
```

Headers carry metadata such as caching, rate limits, and request identifiers.

## Example 11 - Authorization header

```csharp
Client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

Bearer tokens are commonly sent in the `Authorization` header.

## Example 12 - Custom request message

```csharp
using var request = new HttpRequestMessage(HttpMethod.Get, "https://api.example.com/users");
request.Headers.Add("X-Trace-Id", Guid.NewGuid().ToString());
HttpResponseMessage response = await Client.SendAsync(request);
```

`HttpRequestMessage` gives you full control over method, URL, headers, and content.

## Example 13 - Query string

```csharp
string url = $"https://api.example.com/search?q={Uri.EscapeDataString(term)}";
```

Escape query values so spaces and special characters are valid in a URL.

## Example 14 - Deserialize JSON response

```csharp
User? user = await response.Content.ReadFromJsonAsync<User>();
```

`ReadFromJsonAsync<T>` deserializes JSON content directly into a .NET type.

## Example 15 - Typed client

```csharp
public class UserApi(HttpClient http)
{
    public Task<User?> GetAsync(int id) => http.GetFromJsonAsync<User>($"users/{id}");
}
```

A typed client keeps API-specific HTTP logic in one focused class.

## Example 16 - Register typed client

```csharp
services.AddHttpClient<UserApi>(client =>
{
    client.BaseAddress = new Uri("https://api.example.com/");
});
```

`IHttpClientFactory` creates configured clients and manages handlers for production apps.

## Example 17 - Timeout

```csharp
Client.Timeout = TimeSpan.FromSeconds(10);
```

Timeouts prevent requests from hanging forever.

## Example 18 - Cancellation

```csharp
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
HttpResponseMessage response = await Client.GetAsync("https://example.com", cts.Token);
```

Cancellation tokens let callers stop waiting for work they no longer need.

## Example 19 - Error handling result

```csharp
if (!response.IsSuccessStatusCode)
{
    return Result.Failure($"HTTP {(int)response.StatusCode}");
}
```

Checking `IsSuccessStatusCode` lets you convert HTTP failures into your own domain result.

## Example 20 - User agent

```csharp
Client.DefaultRequestHeaders.UserAgent.ParseAdd("TutorialApp/1.0");
```

Some APIs require a user agent so they can identify calling applications.

Helper records used above:

```csharp
public record User(int Id, string Name);
public record Result(bool Success, string? Error)
{
    public static Result Failure(string error) => new(false, error);
}
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `client.BaseAddress = new Uri("https://api.example.com/");` | Sets a common base URL for relative requests. |
| 22 | `await client.GetAsync("users")` | Sends a GET request to a relative endpoint. |
| 23 | `await client.GetFromJsonAsync<User>("users/1")` | Fetches JSON and deserializes it in one call. |
| 24 | `await client.PostAsJsonAsync("users", user)` | Sends an object as a JSON POST body. |
| 25 | `await client.PutAsJsonAsync("users/1", user)` | Sends an object as a JSON PUT body. |
| 26 | `await client.PatchAsync(url, content)` | Sends a PATCH request with custom content. |
| 27 | `await client.DeleteAsync("users/1")` | Sends a DELETE request. |
| 28 | `new StringContent(json, Encoding.UTF8, "application/json")` | Creates explicit JSON request content. |
| 29 | `new FormUrlEncodedContent(values)` | Sends form-style key/value data. |
| 30 | `new MultipartFormDataContent()` | Builds multipart requests such as file uploads. |
| 31 | `request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"))` | Asks the server for JSON. |
| 32 | `request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", token)` | Adds bearer token authentication. |
| 33 | `request.Headers.Add("X-Request-Id", id)` | Sends a custom header. |
| 34 | `response.StatusCode` | Reads the exact HTTP status code. |
| 35 | `response.IsSuccessStatusCode` | Checks whether the status is in the success range. |
| 36 | `await response.Content.ReadAsByteArrayAsync()` | Reads response content as bytes. |
| 37 | `await response.Content.ReadAsStreamAsync()` | Reads response content as a stream. |
| 38 | `await response.Content.ReadFromJsonAsync<User>()` | Deserializes JSON content from the response. |
| 39 | `response.Headers.RetryAfter` | Reads retry guidance from rate-limited APIs. |
| 40 | `Uri.EscapeDataString(term)` | Safely encodes query string values. |
| 41 | `$"users?page={page}&pageSize={size}"` | Builds a simple query string from numeric values. |
| 42 | `services.AddHttpClient()` | Registers the HTTP client factory. |
| 43 | `services.AddHttpClient<UserApi>()` | Registers a typed client. |
| 44 | `services.AddHttpClient("github", client => { })` | Registers a named client. |
| 45 | `factory.CreateClient("github")` | Creates a named configured client. |
| 46 | `client.Timeout = TimeSpan.FromSeconds(30)` | Sets a client-level timeout. |
| 47 | `await client.SendAsync(request, token)` | Sends a custom request with cancellation. |
| 48 | `new HttpRequestException("Failed")` | Represents HTTP request failure. |
| 49 | `catch (TaskCanceledException)` | Often catches request timeout or cancellation. |
| 50 | `if (response.StatusCode == HttpStatusCode.Unauthorized)` | Handles authentication failure explicitly. |
| 51 | `if (response.StatusCode == HttpStatusCode.TooManyRequests)` | Handles rate limiting explicitly. |
| 52 | `return Results.NotFound();` | Web APIs commonly translate missing data to HTTP 404. |
| 53 | `return Results.Ok(value);` | Web APIs commonly translate success data to HTTP 200. |
| 54 | `ProblemDetails` | Standard shape for machine-readable HTTP API errors. |
| 55 | `HttpMethod.Post` | Represents the POST method as a reusable object. |
| 56 | `new HttpMethod("PATCH")` | Creates an HTTP method object not covered by older helpers. |
| 57 | `response.Content.Headers.ContentType` | Reads metadata about response content. |
| 58 | `response.RequestMessage?.RequestUri` | Inspects the request URI associated with a response. |
| 59 | `client.DefaultRequestHeaders.UserAgent.ParseAdd("App/1.0")` | Adds a user agent header. |
| 60 | `using var response = await client.GetAsync(url);` | Disposes response resources after reading them. |
