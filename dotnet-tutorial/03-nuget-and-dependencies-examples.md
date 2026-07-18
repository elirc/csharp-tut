# 03 - NuGet and Dependencies Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `dotnet add package Humanizer` | Adds a package reference. |
| 2 | `dotnet remove package Humanizer` | Removes a package reference. |
| 3 | `dotnet list package` | Lists referenced packages. |
| 4 | `dotnet list package --outdated` | Shows available updates. |
| 5 | `dotnet list package --vulnerable` | Shows known vulnerable packages. |
| 6 | `PackageReference` | Project file package entry. |
| 7 | `Version="1.2.3"` | Package version. |
| 8 | `transitive dependency` | Dependency brought by another package. |
| 9 | `nuget.org` | Default public package feed. |
| 10 | `nuget.config` | Configures package sources. |
| 11 | `dotnet nuget list source` | Lists package feeds. |
| 12 | `dotnet nuget add source` | Adds a package source. |
| 13 | `packages.lock.json` | Optional lock file for dependencies. |
| 14 | `Directory.Packages.props` | Centralizes package versions. |
| 15 | `PrivateAssets="all"` | Prevents dependency flowing transitively. |
| 16 | `IncludeAssets` | Controls consumed package assets. |
| 17 | `analyzer package` | Package that adds code analysis. |
| 18 | `source generator` | Package that generates code at compile time. |
| 19 | `dotnet pack` | Creates a NuGet package. |
| 20 | `dotnet nuget push` | Publishes a package to a feed. |
