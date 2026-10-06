# C# From Basics to Mid-Level

A practical, no-fluff path from "I can write basic code" to "I can be trusted on a real C# team doing CRUD/backend work."

This tutorial assumes you already understand general programming concepts: variables, loops, functions, and what an object roughly is in *some* language. It does **not** assume you know C# or .NET. It focuses on what you actually use day-to-day in typical companies building web APIs, services, and database-backed apps. Niche/esoteric features are deliberately skipped or only mentioned.

## How to use this

Read in order. Each chapter builds on the last. **Type the code yourself** — do not just read it. Most chapters end with exercises; do them. You learn C# by fighting the compiler, not by nodding along.

Every code sample is meant to be runnable. Chapter 0 shows you how to create and run a project.

Chapters 00–18 each have a companion `NN-topic-examples.md` file (20 worked snippets plus a table of 40 one-liners) for repetition; chapters 19–24 have no examples file. [Chapter 24](./24-learning-with-examples-to-mid-level.md) explains how to work through them in passes.

## Sibling tracks in this repo

The same lesson + `-examples.md` format continues in five other folders, each with its own README and chapters 00–17: [dotnet-tutorial/](./dotnet-tutorial/README.md) (the platform: CLI, hosting, DI, EF Core, deployment), [javascript-tutorial/](./javascript-tutorial/README.md), [nodejs-tutorial/](./nodejs-tutorial/README.md), [react-tutorial/](./react-tutorial/README.md) and [typescript-tutorial/](./typescript-tutorial/README.md). For a C# backend path, do this tutorial first, then `dotnet-tutorial/`.

## Table of contents

### Part 1 — The language core
- [00 — Setup & Tooling](./00-setup-and-tooling.md) — install .NET, the `dotnet` CLI, project structure
- [01 — C# Basics](./01-csharp-basics.md) — program structure, console I/O, comments
- [02 — Types & Variables](./02-types-and-variables.md) — value vs reference types, nullability, conversions
- [03 — Control Flow](./03-control-flow.md) — if/switch, loops, jumps
- [04 — Methods](./04-methods.md) — parameters, overloading, ref/out, local functions
- [05 — Classes & Objects](./05-classes-and-objects.md) — fields, properties, constructors, encapsulation
- [06 — Inheritance & Interfaces](./06-inheritance-and-interfaces.md) — polymorphism, abstract classes, interfaces
- [07 — Structs, Records & Enums](./07-structs-records-enums.md) — value types, records, enums

### Part 2 — Working with data
- [08 — Collections](./08-collections.md) — arrays, List, Dictionary, sets, iterators
- [09 — Generics](./09-generics.md) — generic types, methods, constraints
- [10 — LINQ](./10-linq.md) — the single most-used data tool in C#
- [11 — Exceptions & Resources](./11-exceptions-and-resources.md) — try/catch, IDisposable, using

### Part 3 — Modern C# you must know
- [12 — Delegates, Events & Lambdas](./12-delegates-events-lambdas.md) — functions as data
- [13 — Async & Await](./13-async-await.md) — the backbone of modern .NET apps
- [14 — Pattern Matching & Modern Syntax](./14-pattern-matching-and-modern.md)
- [15 — Nullable Reference Types](./15-nullable-reference-types.md) — killing NullReferenceException

### Part 4 — Real-world CRUD engineering
- [16 — File I/O & JSON](./16-file-io-and-json.md)
- [17 — HTTP & Calling APIs](./17-http-and-apis.md)
- [18 — Dependency Injection & Configuration](./18-dependency-injection-and-config.md)
- [19 — Entity Framework Core](./19-entity-framework-core.md) — talking to databases
- [20 — ASP.NET Core Web API](./20-aspnet-core-web-api.md) — building the API
- [21 — Testing](./21-testing.md) — xUnit, mocking, what to test
- [22 — Capstone: A Full CRUD API](./22-capstone-crud-api.md) — tie it all together

### Part 5 — Beyond the tutorial
- [23 — How to Learn From This, and What's Still Missing](./23-how-to-learn-and-whats-missing.md) — a narrative on study method, learning traps, and the real-world skill gaps to close next
- [24 - Learning With Examples Toward Mid-Level C#](./24-learning-with-examples-to-mid-level.md) - how to use the companion example files as deliberate practice and turn tutorial knowledge into job-ready contribution

## What "mid-level" means here

By the end you should be able to:
- Read and write idiomatic modern C# without surprise.
- Build a REST API backed by a real database using ASP.NET Core + EF Core.
- Use async/await correctly (and explain why).
- Use LINQ fluently to query and transform data.
- Apply dependency injection, configuration, and logging the way teams actually do.
- Write unit and integration tests.
- Understand the failure modes that bite juniors: null refs, async deadlocks, leaking resources, mutating shared state.

Let's go. Start with [Chapter 0](./00-setup-and-tooling.md).
