# 00 - .NET Platform and SDK

.NET is a developer platform. C# is one language that runs on it. The .NET SDK includes the compiler, runtime, CLI, templates, build tooling, and libraries needed to create applications.

The runtime runs compiled apps. The SDK builds and publishes them. If you can run an app but cannot create or build one, you may only have the runtime installed.

```bash
dotnet --info
dotnet --list-sdks
dotnet --list-runtimes
```

.NET supports multiple app models: console apps, worker services, web APIs, web apps, class libraries, test projects, desktop apps, mobile apps, and cloud-native services.

SDK versions matter. A repo can pin an SDK with `global.json`. Target frameworks matter too. A project targeting `net8.0`, `net9.0`, or `net10.0` declares which runtime and APIs it expects.

## Exercises

1. Run `dotnet --info`.
2. List installed SDKs and runtimes.
3. Create a console app.
4. Inspect its `.csproj`.
5. Explain SDK versus runtime in your own words.
