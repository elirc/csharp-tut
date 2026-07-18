# 21 — Testing

Mid-level engineers write tests. Not because a rule says so, but because tests let you change code confidently and catch regressions before users do. This chapter covers unit testing with **xUnit**, assertions, mocking dependencies, and integration testing an API. Testing is also where the value of interfaces and DI (Chapters 6, 18) finally pays off.

## Test project setup

Tests live in a separate project referencing the code under test:

```bash
dotnet new xunit -o Shop.Tests
cd Shop.Tests
dotnet add reference ../Shop.Api/Shop.Api.csproj
dotnet test                          # runs all tests
```

**xUnit** is the most common test framework in modern .NET (NUnit and MSTest are alternatives with similar concepts). We'll use xUnit.

## Your first test

```csharp
using Xunit;

public class CalculatorTests
{
    [Fact]                                   // [Fact] = a single test case
    public void Add_TwoPositiveNumbers_ReturnsSum()
    {
        // Arrange — set up inputs and the system under test
        var calculator = new Calculator();

        // Act — perform the operation
        int result = calculator.Add(2, 3);

        // Assert — verify the outcome
        Assert.Equal(5, result);
    }
}
```

The **Arrange / Act / Assert** (AAA) structure is the universal convention — keep tests in those three clear phases. Name tests so they read as a spec: `Method_Scenario_ExpectedResult`.

## `[Fact]` vs `[Theory]`

`[Fact]` is one fixed case. `[Theory]` runs the same test with multiple inputs via `[InlineData]` — perfect for testing many cases without duplication:

```csharp
[Theory]
[InlineData(2, 3, 5)]
[InlineData(-1, 1, 0)]
[InlineData(0, 0, 0)]
[InlineData(100, 200, 300)]
public void Add_VariousInputs_ReturnsExpectedSum(int a, int b, int expected)
{
    var calculator = new Calculator();
    Assert.Equal(expected, calculator.Add(a, b));
}
```

For complex data, use `[MemberData]` / `[ClassData]` to supply test cases from a method/class.

## Assertions

xUnit's built-in assertions cover most needs:

```csharp
Assert.Equal(expected, actual);
Assert.NotEqual(a, b);
Assert.True(condition);
Assert.False(condition);
Assert.Null(obj);
Assert.NotNull(obj);
Assert.Contains("sub", "a substring here");
Assert.Empty(list);
Assert.Single(list);                            // exactly one element
Assert.Equal(3, list.Count);
Assert.IsType<Customer>(obj);                   // exact type
Assert.IsAssignableFrom<IEntity>(obj);

// Floating point — always assert with a tolerance:
Assert.Equal(0.3, result, precision: 5);

// Asserting an exception is thrown:
var ex = Assert.Throws<ArgumentException>(() => calculator.Divide(1, 0));
Assert.Equal("Cannot divide by zero", ex.Message);

// Async exception:
await Assert.ThrowsAsync<NotFoundException>(() => service.GetAsync(999));
```

Many teams add a fluent assertion library (**FluentAssertions** / **Shouldly**) for more readable assertions: `result.Should().Be(5);` `customers.Should().HaveCount(3);`. Nice-to-have, not required.

## What to test (and what not to)

- **Test business logic and behavior**: calculations, validation rules, branching, edge cases, error paths.
- **Test the things that would hurt if they broke** and the tricky bits where bugs hide (boundaries: empty, null, zero, max, negatives).
- **Don't test the framework or trivial code**: getters/setters, plain DTOs, EF itself.
- Aim for tests that are **fast, isolated, deterministic** — no real network, no shared mutable state, no `DateTime.Now` randomness (inject a clock).

Good tests are independent — order shouldn't matter, and one failing test shouldn't cascade.

## Testing code with dependencies — mocking

A unit test should test *one unit* in isolation. When your class depends on a database, email sender, or HTTP API, you don't want the real thing in a unit test. Because you depended on **interfaces** (Chapter 6) and used **DI** (Chapter 18), you can pass in a **test double** instead.

### Hand-written fakes (often enough)

```csharp
public class FakeEmailSender : IEmailSender
{
    public List<(string To, string Body)> Sent { get; } = new();
    public Task SendAsync(string to, string body)
    {
        Sent.Add((to, body));
        return Task.CompletedTask;
    }
}

[Fact]
public async Task PlaceOrder_SendsConfirmationEmail()
{
    var email = new FakeEmailSender();
    var service = new OrderService(new FakeOrderRepository(), email);

    await service.PlaceAsync(new Order { CustomerEmail = "a@b.com" });

    Assert.Single(email.Sent);
    Assert.Equal("a@b.com", email.Sent[0].To);
}
```

### Mocking libraries (Moq / NSubstitute)

For richer setups, a mocking library generates doubles on the fly and lets you configure return values and verify calls. **Moq** is the most widespread:

```csharp
using Moq;

[Fact]
public async Task GetCustomer_WhenNotFound_ReturnsNull()
{
    // Arrange: create a mock repository and configure its behavior
    var repo = new Mock<ICustomerRepository>();
    repo.Setup(r => r.GetByIdAsync(42, It.IsAny<CancellationToken>()))
        .ReturnsAsync((Customer?)null);

    var service = new CustomerService(repo.Object);

    // Act
    var result = await service.GetAsync(42);

    // Assert
    Assert.Null(result);
    repo.Verify(r => r.GetByIdAsync(42, It.IsAny<CancellationToken>()), Times.Once);
}
```

- `Setup(...).Returns/ReturnsAsync(...)` — stub a method's return value.
- `It.IsAny<T>()` — match any argument.
- `Verify(...)` — assert a method was (or wasn't) called, optionally how many times.

(NSubstitute is a popular alternative with cleaner syntax: `repo.GetByIdAsync(42, default).Returns((Customer?)null);`.) Don't over-mock — if a test mocks five things and asserts on the mocks rather than real behavior, it's testing the wiring, not the logic. Prefer testing observable behavior.

## Integration testing an API

Unit tests check logic in isolation; **integration tests** verify the pieces work together — real routing, model binding, DI, even a real (test) database. ASP.NET Core provides `WebApplicationFactory<T>` to spin up your app in-memory and hit it with `HttpClient`:

```csharp
public class CustomersApiTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly HttpClient _client;
    public CustomersApiTests(WebApplicationFactory<Program> factory)
        => _client = factory.CreateClient();

    [Fact]
    public async Task Post_then_Get_ReturnsCreatedCustomer()
    {
        // Create
        var create = new CreateCustomerRequest("Sam", "sam@example.com");
        var post = await _client.PostAsJsonAsync("/api/customers", create);
        Assert.Equal(HttpStatusCode.Created, post.StatusCode);

        var created = await post.Content.ReadFromJsonAsync<CustomerDto>();
        Assert.NotNull(created);

        // Read it back
        var get = await _client.GetFromJsonAsync<CustomerDto>($"/api/customers/{created!.Id}");
        Assert.Equal("sam@example.com", get!.Email);
    }

    [Fact]
    public async Task Get_NonexistentCustomer_Returns404()
    {
        var resp = await _client.GetAsync("/api/customers/999999");
        Assert.Equal(HttpStatusCode.NotFound, resp.StatusCode);
    }
}
```

(For `WebApplicationFactory<Program>` to find your entry point, the API project's `Program` must be accessible — adding `public partial class Program { }` at the bottom of `Program.cs`, or referencing it, makes top-level-statement programs testable.)

For the database in integration tests, point the app at a disposable database: **SQLite in-memory**, the EF in-memory provider (limited), or — most realistic — a real database in a container via **Testcontainers**. Override the DI registration in the factory to swap the connection string.

## The testing pyramid

A healthy suite has many fast **unit tests** (logic in isolation), fewer **integration tests** (components together), and a small number of **end-to-end** tests (the whole system). Lean on unit tests for breadth and speed; use integration tests for the critical paths where wiring matters.

```
        /\        few   end-to-end (slow, brittle, high confidence)
       /  \
      /----\      some  integration (API + DB + DI)
     /      \
    /--------\    many  unit (fast, isolated, business logic)
```

## Practical habits

- Run `dotnet test` locally before pushing; CI should run it on every PR.
- Write a failing test that reproduces a bug *before* fixing it (regression test) — then fix and watch it pass.
- Keep tests readable: clear AAA, descriptive names, one logical assertion focus per test.
- Don't chase 100% coverage as a goal; cover the logic that matters. Coverage is a hint, not a target.
- Make tests deterministic: inject `IClock`/`TimeProvider` instead of `DateTime.Now`, seed randomness, avoid real I/O in unit tests.

## Exercises

1. Write `[Theory]` tests for a `Calculator.Add/Subtract/Divide`, including a `[Fact]` that asserts dividing by zero throws.
2. Test `OrderService.PlaceAsync` with a `Mock<IOrderRepository>` and `Mock<IEmailSender>`: verify the order is saved and a confirmation email is sent.
3. Write an integration test using `WebApplicationFactory<Program>` that POSTs a customer and GETs it back, asserting status codes and the round-tripped data.
4. Find a method with branching (e.g., a discount calculator) and write tests covering every branch and boundary value.

---

**Next:** [22 — Capstone: A Full CRUD API](./22-capstone-crud-api.md)
