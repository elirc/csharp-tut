# 00 — Setup & Tooling

Before any C#, you need the SDK and a way to run code. This chapter gets you to a running program and explains the tooling you'll touch every day.

## What is .NET vs C#?

- **C#** is the language.
- **.NET** is the platform: the runtime (CLR) that executes your code, plus the huge standard library (BCL — Base Class Library) of built-in types like `List<T>`, `String`, `HttpClient`, etc.

You write C#, it compiles to **IL** (intermediate language) inside a **.dll/.exe assembly**, and the **CLR** JIT-compiles that to machine code and runs it, with a **garbage collector (GC)** managing memory for you. You almost never free memory manually.

Modern .NET (formerly ".NET Core", now just **.NET 8**, **.NET 9**, etc.) is cross-platform (Windows/Linux/macOS) and is what all new work uses. The old Windows-only **.NET Framework 4.x** still exists in legacy enterprises — same C# language, older library/runtime. This tutorial targets modern .NET.

## Install the SDK

Install the latest **.NET SDK** from https://dotnet.microsoft.com/download — today that means **.NET 10 (LTS)**. The samples in this tutorial show `net8.0` and run unchanged on 8.0 or newer, but .NET 8 support ends in November 2026, so set `TargetFramework` to whatever `dotnet new` generates for you. The SDK includes the compiler, runtime, and the `dotnet` CLI.

Verify:

```bash
dotnet --version
dotnet --list-sdks
```

### Editor

Use either:
- **Visual Studio** (Windows, full IDE, free Community edition) — heavy but powerful.
- **VS Code** + the **C# Dev Kit** extension — lighter, cross-platform. Most common today.
- **JetBrains Rider** — excellent, paid.

Any of them gives you IntelliSense (autocomplete), debugging, and refactoring. Get comfortable with the debugger early — breakpoints beat `Console.WriteLine` for understanding code.

## Your first project with the `dotnet` CLI

The `dotnet` CLI is how you create, build, run, and test projects from a terminal. Learn it even if you use an IDE — CI/CD and servers only have the CLI.

```bash
# Create a new console app in a folder named "Hello"
dotnet new console -o Hello
cd Hello

# Run it
dotnet run
```

You'll see `Hello, World!`. That's the default template.

### What got created

```
Hello/
├── Hello.csproj      <- the project file (build config, dependencies)
├── Program.cs        <- your code
└── obj/ , bin/       <- build output (generated; don't commit, gitignore them)
```

**`Hello.csproj`** is XML describing the project:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>
</Project>
```

Key properties:
- `TargetFramework` — which .NET version you build against (`net8.0` here; `net10.0` on a current SDK).
- `ImplicitUsings` — auto-imports common namespaces (`System`, `System.Collections.Generic`, etc.) so you don't write `using System;` everywhere.
- `Nullable` — enables nullable reference type checking (Chapter 15). Keep it `enable`.

**`Program.cs`** with the modern "top-level statements" template is just:

```csharp
Console.WriteLine("Hello, World!");
```

No `class`, no `Main` — the compiler generates them. We'll see the full form in Chapter 1.

## Essential CLI commands

```bash
dotnet new console -o MyApp     # new console app
dotnet new webapi -o MyApi      # new web API
dotnet new classlib -o MyLib    # new reusable library
dotnet new xunit -o MyTests     # new test project
dotnet new gitignore            # add a .NET .gitignore

dotnet build                    # compile
dotnet run                      # build + run
dotnet test                     # run tests
dotnet watch run                # run and auto-restart on file change (great for dev)

dotnet add package Newtonsoft.Json   # add a NuGet dependency
dotnet add reference ../MyLib/MyLib.csproj  # reference another project
```

## NuGet — package management

**NuGet** is C#'s package registry (like npm/pip/Maven). `dotnet add package <name>` records the dependency in your `.csproj`:

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
</ItemGroup>
```

`dotnet restore` downloads them (runs automatically on build). You'll add packages constantly — EF Core, serializers, test libraries, etc.

## Solutions

A **solution** (`.sln`) groups multiple projects (e.g., the API, a class library, and a test project) so you build/open them together.

```bash
dotnet new sln -n MyShop
dotnet sln add MyApi/MyApi.csproj
dotnet sln add MyTests/MyTests.csproj
```

Typical real layout:

```
MyShop.sln
src/
  MyShop.Api/          <- web API project
  MyShop.Core/         <- domain models + business logic (class library)
  MyShop.Data/         <- database access (class library)
tests/
  MyShop.Tests/        <- xunit tests
```

You don't need this structure yet — a single console app is fine for learning the language. We'll build up to it in Part 4.

## Running the tutorial's examples

For chapters 1–15, create one scratch console app and paste examples into `Program.cs`:

```bash
dotnet new console -o Scratch
cd Scratch
dotnet watch run    # auto-reruns as you edit
```

---

**Next:** [01 — C# Basics](./01-csharp-basics.md)
