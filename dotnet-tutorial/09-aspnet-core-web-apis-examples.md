# 09 - ASP.NET Core Web APIs Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `WebApplication.CreateBuilder(args)` | Creates web app builder. |
| 2 | `builder.Services` | Registers web dependencies. |
| 3 | `builder.Build()` | Builds web app. |
| 4 | `app.MapGet()` | Maps GET endpoint. |
| 5 | `app.MapPost()` | Maps POST endpoint. |
| 6 | `Results.Ok(value)` | Returns HTTP 200. |
| 7 | `Results.Created(uri, value)` | Returns HTTP 201. |
| 8 | `Results.NoContent()` | Returns HTTP 204. |
| 9 | `Results.NotFound()` | Returns HTTP 404. |
| 10 | `Results.BadRequest()` | Returns HTTP 400. |
| 11 | `[ApiController]` | Controller API conventions. |
| 12 | `[HttpGet("{id}")]` | Controller route for GET by id. |
| 13 | `[FromBody]` | Binds request body. |
| 14 | `[FromQuery]` | Binds query string. |
| 15 | `[FromRoute]` | Binds route values. |
| 16 | `ProblemDetails` | Standard error response shape. |
| 17 | `UseExceptionHandler` | Centralized exception handling. |
| 18 | `OpenAPI` | Machine-readable API description. |
| 19 | `middleware pipeline` | Ordered request processing chain. |
| 20 | `endpoint filters` | Minimal API cross-cutting endpoint logic. |
