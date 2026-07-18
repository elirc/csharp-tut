# 15 - Security and Secrets Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `dotnet user-secrets init` | Enables local development secrets. |
| 2 | `dotnet user-secrets set "ApiKey" "value"` | Stores a local secret. |
| 3 | `dotnet user-secrets list` | Lists local secrets. |
| 4 | `ASPNETCORE_ENVIRONMENT` | Controls environment-specific behavior. |
| 5 | `UseHttpsRedirection()` | Redirects HTTP to HTTPS. |
| 6 | `UseAuthentication()` | Enables authentication middleware. |
| 7 | `UseAuthorization()` | Enables authorization middleware. |
| 8 | `[Authorize]` | Requires authenticated/authorized user. |
| 9 | `[AllowAnonymous]` | Allows anonymous access. |
| 10 | `policy authorization` | Rule-based authorization. |
| 11 | `claims` | User facts from identity provider. |
| 12 | `roles` | Role-based authorization input. |
| 13 | `JWT bearer` | Common API auth token style. |
| 14 | `OAuth/OIDC` | Common delegated identity protocols. |
| 15 | `Data Protection` | .NET APIs for protecting data. |
| 16 | `parameterized SQL` | Prevents SQL injection. |
| 17 | `model validation` | Rejects invalid request data. |
| 18 | `secret scanning` | Detects committed secrets. |
| 19 | `dotnet list package --vulnerable` | Checks vulnerable dependencies. |
| 20 | `least privilege` | Give apps only permissions they need. |
