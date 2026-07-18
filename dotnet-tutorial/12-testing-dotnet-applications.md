# 12 - Testing .NET Applications

.NET testing usually combines unit tests, integration tests, and sometimes end-to-end tests.

Create a test project:

```bash
dotnet new xunit -n MyApp.Tests
dotnet add MyApp.Tests reference MyApp
dotnet test
```

Unit tests are best for pure logic and services. Integration tests are best for real app boundaries such as ASP.NET Core endpoints, EF Core behavior, configuration, and DI wiring.

Example xUnit test:

```csharp
public class CalculatorTests
{
    [Fact]
    public void Add_ReturnsSum()
    {
        Assert.Equal(5, Calculator.Add(2, 3));
    }
}
```

Good tests cover happy paths, edge cases, validation failures, not-found cases, and regressions from bugs.

## Exercises

1. Create an xUnit test project.
2. Test a pure function.
3. Test a service with a fake dependency.
4. Add an ASP.NET Core integration test.
5. Run tests from the CLI.
