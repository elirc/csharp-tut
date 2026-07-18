# 03 - NuGet and Dependencies

NuGet is the package manager for .NET. Packages provide libraries, tools, analyzers, source generators, and framework integrations.

```bash
dotnet add package Humanizer
dotnet list package
dotnet remove package Humanizer
```

Packages become `PackageReference` entries in the project file. Restore resolves dependencies and writes lock/intermediate data under `obj`.

Dependency management is a real engineering skill. You need to understand package versions, transitive dependencies, vulnerability warnings, private feeds, package sources, and update risk.

For larger repos, central package management keeps versions in one file.

## Exercises

1. Add a package.
2. List packages.
3. Remove a package.
4. Inspect the project file.
5. Check for outdated packages.
