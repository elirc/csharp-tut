# 17 - How to Learn .NET This Way

.NET is not just C# syntax. It is a platform: SDK, CLI, project system, package ecosystem, hosting model, configuration, dependency injection, logging, web framework, data access, testing tools, deployment options, and production diagnostics.

This folder uses the same paired method as the other tutorials. The lesson file explains the concept. The examples file gives repeated contact with commands, APIs, settings, and platform vocabulary. Your job is to turn those examples into working habits.

## The learning loop

For each concept:

1. Read the lesson.
2. Type 10 examples from the companion file.
3. Run at least one command or create one tiny project.
4. Inspect the generated files.
5. Change one setting and observe the result.
6. Explain what changed and why.
7. Recreate the pattern later from memory.

This matters because .NET fluency is operational. You need to know what command to run, where the setting lives, how DI wires the app, why logs appear, where config comes from, and how the project builds.

## What to focus on

Focus first on the CLI and project files. If you understand `dotnet new`, `build`, `run`, `test`, `publish`, `.csproj`, and `.sln`, you can orient yourself in most .NET repos.

Then focus on the built-in app services: configuration, options, dependency injection, and logging. These appear everywhere. ASP.NET Core, workers, EF Core, HTTP clients, and tests all depend on them.

Then focus on production boundaries: database access, HTTP integration, tests, deployment, observability, and security. These are the areas that turn a local demo into a service a team can trust.

## What examples cannot teach alone

Commands and snippets are useful, but real .NET work is cross-cutting. A feature might require a DTO, validation, service logic, DbContext query, migration, endpoint, test, log, config value, and deployment setting. The capstone exists so you practice moving across those layers.

Do not stop at reading generated code. Create services. Break configuration. Change lifetimes. Run tests. Publish the app. Inspect logs. Watch what happens when the database is unavailable. That is how platform knowledge becomes real.

## Gaps to expand next

After this tutorial, expand into:

- SQL and database performance.
- Authentication and authorization in depth.
- Docker and container deployment.
- CI/CD pipelines.
- Cloud hosting, especially Azure if your target jobs use Microsoft stacks.
- OpenTelemetry and production observability.
- Distributed systems basics: queues, retries, idempotency, and eventual consistency.
- Advanced EF Core performance and migrations.
- Background processing with queues.
- Package authoring and internal NuGet feeds.
- Legacy .NET Framework maintenance.

## What contribution readiness looks like

You are ready to contribute when you can clone a .NET repo, restore it, build it, run tests, find the startup project, understand configuration, trace a request through DI into services and data access, make a scoped change, test it, and explain the operational impact.

Good .NET developers are not just good at writing classes. They understand how the application is assembled, configured, observed, secured, tested, and shipped.

The examples build platform vocabulary. The capstone builds coordination. Real services build judgment.

---

Back to the [README index](./README.md).
