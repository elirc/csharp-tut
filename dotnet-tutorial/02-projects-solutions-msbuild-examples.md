# 02 - Projects, Solutions, and MSBuild Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `.csproj` | C# project file. |
| 2 | `.sln` | Solution file grouping projects. |
| 3 | `<Project Sdk="Microsoft.NET.Sdk">` | SDK-style project root. |
| 4 | `<TargetFramework>net8.0</TargetFramework>` | Target framework. |
| 5 | `<OutputType>Exe</OutputType>` | Builds an executable. |
| 6 | `<Nullable>enable</Nullable>` | Enables nullable analysis. |
| 7 | `<ImplicitUsings>enable</ImplicitUsings>` | Adds common usings automatically. |
| 8 | `<PackageReference Include="Dapper" Version="2.1.66" />` | Adds a NuGet dependency. |
| 9 | `<ProjectReference Include="..\\Core\\Core.csproj" />` | References another project. |
| 10 | `Directory.Build.props` | Shared MSBuild properties for a repo. |
| 11 | `Directory.Packages.props` | Central package version management file. |
| 12 | `Debug` | Default development build configuration. |
| 13 | `Release` | Optimized build configuration. |
| 14 | `bin/Debug/net8.0` | Typical debug output path. |
| 15 | `obj/project.assets.json` | Restore dependency graph file. |
| 16 | `dotnet msbuild` | Runs MSBuild through the CLI. |
| 17 | `dotnet build /bl` | Creates a binary build log. |
| 18 | `ItemGroup` | Groups project items and references. |
| 19 | `PropertyGroup` | Groups project properties. |
| 20 | `TreatWarningsAsErrors` | Can make compiler warnings fail the build. |
