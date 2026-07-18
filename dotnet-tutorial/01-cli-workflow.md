# 01 - CLI Workflow

The `dotnet` CLI is the everyday cross-platform toolchain for .NET development. It creates projects, restores packages, builds code, runs apps, runs tests, publishes output, and manages solutions.

```bash
dotnet new console -n HelloDotnet
cd HelloDotnet
dotnet run
```

Most commands follow this shape:

```bash
dotnet <command> [arguments] [options]
```

Common commands are `new`, `restore`, `build`, `run`, `test`, `publish`, `clean`, and `sln`.

The CLI is also useful in CI because it works without an IDE. A professional .NET developer should be comfortable building and testing from the terminal.

## Exercises

1. Create a console app from the CLI.
2. Build it without running.
3. Run it.
4. Clean build output.
5. Publish a release build.
