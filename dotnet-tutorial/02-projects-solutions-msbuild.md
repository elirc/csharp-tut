# 02 - Projects, Solutions, and MSBuild

A `.csproj` file describes one project. A `.sln` file groups projects. MSBuild is the build engine underneath the CLI and project system.

Modern .NET projects use SDK-style project files:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```

Project files can include package references, project references, properties, items, and build settings.

Solutions are useful once you have multiple projects: API, domain library, data library, tests, worker, and shared packages.

## Exercises

1. Create a solution.
2. Add an API, class library, and test project.
3. Add a project reference.
4. Inspect the generated `.csproj`.
5. Change a project property and rebuild.
