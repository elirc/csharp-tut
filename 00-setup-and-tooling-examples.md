# 00 - Setup & Tooling Examples

These examples reinforce the setup, project, CLI, NuGet, and solution workflow from the lesson.

## Example 1 - Check the installed SDK

```powershell
dotnet --version
```

This prints the default .NET SDK version your terminal will use for new projects and builds.

## Example 2 - List all installed SDKs

```powershell
dotnet --list-sdks
```

Use this when a project requires a specific SDK and you want to know whether it is installed.

## Example 3 - Create a console app

```powershell
dotnet new console -n HelloTools
```

The `-n` option names both the project folder and the generated `.csproj` file.

## Example 4 - Run a project from its folder

```powershell
cd HelloTools
dotnet run
```

`dotnet run` restores, builds, and starts the app, which is convenient while learning.

## Example 5 - Build without running

```powershell
dotnet build
```

Building checks whether the code compiles and writes compiled output under `bin`.

## Example 6 - Clean build output

```powershell
dotnet clean
```

This removes generated build files, which can help when you want a fresh build.

## Example 7 - Create a project in the current folder

```powershell
mkdir ScratchApp
cd ScratchApp
dotnet new console
```

Leaving off `-n` tells the CLI to create the project in the current directory.

## Example 8 - Show project templates

```powershell
dotnet new list
```

Templates are starting points such as `console`, `classlib`, `webapi`, and `xunit`.

## Example 9 - Add a NuGet package

```powershell
dotnet add package Humanizer
```

This edits the project file and restores the package so your code can reference it.

## Example 10 - Remove a NuGet package

```powershell
dotnet remove package Humanizer
```

Removing a package keeps your project file from carrying dependencies you no longer use.

## Example 11 - Restore dependencies explicitly

```powershell
dotnet restore
```

Most commands restore automatically, but explicit restore is useful in CI and troubleshooting.

## Example 12 - Create a solution file

```powershell
dotnet new sln -n TutorialWorkspace
```

A solution groups multiple projects so you can build and manage them together.

## Example 13 - Add a project to a solution

```powershell
dotnet sln add HelloTools/HelloTools.csproj
```

After this, solution-level commands know that the project belongs to the workspace.

## Example 14 - Add a class library

```powershell
dotnet new classlib -n SharedLogic
dotnet sln add SharedLogic/SharedLogic.csproj
```

Class libraries hold reusable code that other projects can reference.

## Example 15 - Reference one project from another

```powershell
dotnet add HelloTools/HelloTools.csproj reference SharedLogic/SharedLogic.csproj
```

Project references let one project use types from another project in the same solution.

## Example 16 - Run a specific project

```powershell
dotnet run --project HelloTools/HelloTools.csproj
```

This is handy when your terminal is at the solution root instead of inside a project folder.

## Example 17 - Create a debug build

```powershell
dotnet build -c Debug
```

Debug builds keep extra information that makes breakpoints and stack traces easier to use.

## Example 18 - Create a release build

```powershell
dotnet build -c Release
```

Release builds optimize the compiled app and are closer to what you would deploy.

## Example 19 - Publish a console app

```powershell
dotnet publish -c Release -o ./publish
```

Publishing collects the files needed to run the app into one output folder.

## Example 20 - Inspect the project file

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

The `.csproj` file declares the target framework, project type, compiler defaults, and dependencies.

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `dotnet --info` | Shows SDK, runtime, OS, and environment details for troubleshooting. |
| 22 | `dotnet new console --use-program-main` | Creates a console app with the older explicit `Main` method shape. |
| 23 | `dotnet new gitignore` | Adds a C# friendly `.gitignore` for build outputs and local files. |
| 24 | `dotnet build --no-restore` | Builds without restoring, useful after dependencies are already restored. |
| 25 | `dotnet run --no-build` | Runs the existing build output without compiling again. |
| 26 | `dotnet test` | Runs tests for test projects in the current project or solution. |
| 27 | `dotnet new xunit -n HelloTools.Tests` | Creates a new xUnit test project. |
| 28 | `dotnet add HelloTools.Tests reference HelloTools` | Lets the test project call code from the app project. |
| 29 | `dotnet sln list` | Lists projects currently included in the solution. |
| 30 | `dotnet sln remove OldApp/OldApp.csproj` | Removes a project from the solution without deleting its files. |
| 31 | `dotnet list package` | Shows NuGet packages referenced by the project. |
| 32 | `dotnet list package --outdated` | Checks whether package updates are available. |
| 33 | `dotnet add package Dapper --version 2.1.35` | Installs a specific package version instead of the latest available. |
| 34 | `dotnet nuget locals all --clear` | Clears NuGet caches when restore behaves strangely. |
| 35 | `dotnet format` | Applies standard formatting when the formatter tool is available. |
| 36 | `dotnet watch run` | Rebuilds and reruns the app when source files change. |
| 37 | `dotnet new classlib -n Domain` | Creates a library project for shared domain code. |
| 38 | `dotnet new webapi -n Api` | Creates a starter ASP.NET Core Web API project. |
| 39 | `dotnet publish -r win-x64` | Publishes for a specific runtime target. |
| 40 | `dotnet publish --self-contained false` | Publishes an app that expects .NET to be installed on the machine. |
| 41 | `dotnet publish --self-contained true` | Publishes an app with the runtime included. |
| 42 | `<TargetFramework>net8.0</TargetFramework>` | Targets a specific .NET framework version in the project file. |
| 43 | `<ImplicitUsings>enable</ImplicitUsings>` | Automatically imports common namespaces for newer projects. |
| 44 | `<Nullable>enable</Nullable>` | Turns on nullable reference type analysis. |
| 45 | `<LangVersion>latest</LangVersion>` | Asks the compiler to use the latest C# language version. |
| 46 | `<PackageReference Include="Humanizer" Version="2.14.1" />` | Shows how NuGet packages appear inside a project file. |
| 47 | `<ProjectReference Include="..\Domain\Domain.csproj" />` | Shows how project references appear inside a project file. |
| 48 | `bin/Debug/net8.0/` | The usual folder for debug build output. |
| 49 | `obj/project.assets.json` | A generated restore file that records resolved NuGet dependencies. |
| 50 | `global.json` | Pins the SDK version used by a repo. |
| 51 | `dotnet new globaljson --sdk-version 8.0.100` | Creates a `global.json` file for SDK pinning. |
| 52 | `dotnet workload list` | Lists optional workloads installed for mobile, desktop, or WebAssembly development. |
| 53 | `dotnet workload update` | Updates installed .NET workloads. |
| 54 | `dotnet tool list -g` | Lists globally installed .NET CLI tools. |
| 55 | `dotnet tool install -g dotnet-ef` | Installs a global CLI tool. |
| 56 | `dotnet tool update -g dotnet-ef` | Updates a global CLI tool. |
| 57 | `dotnet dev-certs https --trust` | Trusts the local HTTPS development certificate. |
| 58 | `dotnet user-secrets init` | Enables local user secrets for a project. |
| 59 | `dotnet user-secrets list` | Lists local secrets for the current project. |
| 60 | `dotnet --help` | Shows available CLI commands and options when you forget the exact syntax. |
