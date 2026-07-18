# 12 - Testing .NET Applications Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `dotnet new xunit` | Creates xUnit test project. |
| 2 | `dotnet test` | Runs tests. |
| 3 | `[Fact]` | Single test case. |
| 4 | `[Theory]` | Parameterized test. |
| 5 | `[InlineData]` | Provides theory data. |
| 6 | `Assert.Equal(expected, actual)` | Checks equality. |
| 7 | `Assert.True(value)` | Checks boolean true. |
| 8 | `Assert.Throws<T>()` | Checks exception. |
| 9 | `await Assert.ThrowsAsync<T>()` | Checks async exception. |
| 10 | `WebApplicationFactory<T>` | ASP.NET Core integration test host. |
| 11 | `TestServer` | In-memory test server. |
| 12 | `HttpClient in tests` | Calls test API endpoints. |
| 13 | `fake dependency` | Simple test implementation. |
| 14 | `mock` | Configurable test double. |
| 15 | `Moq` | Popular mocking library. |
| 16 | `NSubstitute` | Popular mocking library. |
| 17 | `FluentAssertions` | Fluent assertion library. |
| 18 | `test fixture` | Shared setup object. |
| 19 | `Arrange Act Assert` | Common test structure. |
| 20 | `CI test run` | Tests should run automatically on PRs. |
