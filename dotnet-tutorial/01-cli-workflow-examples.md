# 01 - CLI Workflow Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `dotnet new console -n App` | Creates a console project. |
| 2 | `dotnet restore` | Restores NuGet packages. |
| 3 | `dotnet build` | Compiles the project. |
| 4 | `dotnet run` | Builds and runs the app. |
| 5 | `dotnet clean` | Removes build outputs. |
| 6 | `dotnet test` | Runs tests. |
| 7 | `dotnet publish -c Release` | Publishes release output. |
| 8 | `dotnet watch run` | Reruns when files change. |
| 9 | `dotnet format` | Formats code when available. |
| 10 | `dotnet new sln -n Workspace` | Creates a solution file. |
| 11 | `dotnet sln add App/App.csproj` | Adds project to a solution. |
| 12 | `dotnet run --project App` | Runs a specific project. |
| 13 | `dotnet build -c Release` | Builds release configuration. |
| 14 | `dotnet build --no-restore` | Builds without restoring first. |
| 15 | `dotnet run --no-build` | Runs existing build output. |
| 16 | `dotnet help` | Shows CLI help. |
| 17 | `dotnet new search worker` | Searches templates. |
| 18 | `dotnet tool list -g` | Lists global tools. |
| 19 | `dotnet dev-certs https --trust` | Trusts local HTTPS dev cert. |
| 20 | `dotnet nuget locals all --clear` | Clears NuGet caches. |
