# 13 - Publishing and Deployment Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `dotnet publish -c Release` | Publishes release output. |
| 2 | `-o ./publish` | Chooses publish output folder. |
| 3 | `--self-contained true` | Includes .NET runtime. |
| 4 | `--self-contained false` | Requires runtime installed on host. |
| 5 | `-r linux-x64` | Publishes for Linux x64 runtime. |
| 6 | `-r win-x64` | Publishes for Windows x64 runtime. |
| 7 | `PublishSingleFile=true` | Bundles output into one file. |
| 8 | `PublishTrimmed=true` | Removes unused code where safe. |
| 9 | `ReadyToRun` | Precompiles some code for startup. |
| 10 | `NativeAOT` | Ahead-of-time native publishing option. |
| 11 | `Dockerfile` | Container build recipe. |
| 12 | `docker build` | Builds container image. |
| 13 | `docker run` | Runs container image. |
| 14 | `ASPNETCORE_URLS` | Configures web app binding URLs. |
| 15 | `environment variables` | Common production configuration source. |
| 16 | `health check endpoint` | Deployment readiness signal. |
| 17 | `migration strategy` | Plan for database schema changes. |
| 18 | `rollback` | Plan to recover from bad deployment. |
| 19 | `CI/CD pipeline` | Builds, tests, and deploys automatically. |
| 20 | `staging slot` | Pre-production deployment target. |
