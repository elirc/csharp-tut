# 24 - Learning With Examples Toward Mid-Level C#

This tutorial now has two layers. The first layer is the main path: chapters 0 through 22 take you from setup and syntax into ASP.NET Core, EF Core, testing, and a CRUD API capstone. The second layer is the companion example files for chapters 0 through 18. Those files are not just extra material. They are the practice field.

The main chapters teach the shape of the idea. The example files give you repeated contact with the idea from slightly different angles. That distinction matters. A beginner often thinks they need one perfect explanation. In practice, programming skill is built by seeing the same concept in many small situations until the pattern becomes familiar enough that your hands can use it without panic.

That is the point of this method: read the concept, see many examples, type and alter those examples, then use the concept in a small piece of real work. Reading gives you orientation. Examples give you pattern recognition. Exercises give you recall. Projects give you judgment.

## How To Use The Tutorial And Example Files

Do not treat the companion example files like reference pages to skim once. Their value is repetition. You should work through them in passes.

On the first pass, read the main chapter slowly. Do not try to memorize every detail. Your goal is to understand the vocabulary: what the feature is called, why it exists, and what kind of problem it solves. For example, after the generics chapter, you should be able to say, "Generics let me write one type or method that works with caller-chosen types while preserving compile-time safety." That kind of sentence is enough for the first pass.

On the second pass, open the matching `*-examples.md` file and type a selection of examples by hand. Do not copy and paste. Typing is slower, but slow is doing work here. It forces you to notice punctuation, casing, generic angle brackets, nullable annotations, method signatures, and the tiny details that make C# feel strict at first. When something fails to compile, resist the urge to immediately paste the error into a search box. Read the error. Guess what it means. Then fix it.

On the third pass, change the examples. This is where the real learning starts. If an example uses `List<string>`, make it `List<int>`. If it uses `First`, try `Single` and see what changes. If it safely handles null with `??`, remove the fallback and observe the compiler warning. If an async example awaits two tasks sequentially, rewrite it with `Task.WhenAll`. Small variations turn passive recognition into active control.

On the fourth pass, use the concept in a tiny task of your own. After collections, build a contact list in memory. After file I/O and JSON, save that contact list to disk. After HTTP, call a public API and map the response into records. After dependency injection, split the program into a service and an interface. The task can be ugly. It can be small. It just has to be yours.

## What This Method Is Good At

This method is excellent for building fluency. C# has a lot of surface area: nullable references, LINQ, async, records, pattern matching, DI, configuration, HTTP, JSON, EF Core, tests. If you only read one explanation of each, you will recognize them later but struggle to write them. The many-example approach reduces that gap.

It is also good for reducing intimidation. A feature like LINQ can seem magical when you see one dense query. But after enough small examples of `Where`, `Select`, `GroupBy`, `ToDictionary`, and deferred execution, the magic fades into a mental model: a sequence comes in, a sequence or value comes out, and each operator has a job. That is exactly what you want. Tools become less mysterious when you have seen them behave in many ordinary cases.

The method also teaches vocabulary. This matters more than beginners expect. On a team, you need to understand phrases like "make that nullable," "inject the dependency," "materialize the query," "use a DTO," "add a guard clause," "this should be async all the way," or "do not leak entities from the API." Knowing the words lets you participate in review conversations and ask sharper questions.

Finally, this method creates a trail you can revisit. The companion files are compact drills. When you forget how `Dictionary.TryGetValue` works, or what an event declaration looks like, you do not have to reread the full chapter. You can jump into the examples, refresh the pattern, and move on.

## What This Method Is Not Enough For

Examples are necessary, but they are not sufficient. A snippet is too small to teach design pressure. It usually has no messy requirements, no naming tradeoffs, no old code around it, no failing test, no production data, no teammate asking why you chose that approach. That means snippets can teach syntax and local reasoning, but they cannot fully teach engineering judgment.

This is the danger: you can become very good at understanding small examples and still freeze when asked to make a change in a real application. A real ticket does not say "practice `GroupBy`." It says something like, "Show managers a weekly summary of overdue tasks, grouped by project, excluding archived projects, with pagination, and do not break the existing mobile client." Suddenly the work is not just LINQ. It is requirements, data modeling, API shape, compatibility, tests, performance, and communication.

So use the examples as reps, not as the whole workout. They should prepare you for projects. They should not replace projects.

## A Practical Learning Loop

A strong loop for each chapter looks like this:

1. Read the main chapter.
2. Type 10 to 15 examples from the companion file.
3. Modify 5 examples without looking at the answer.
4. Do the chapter exercises.
5. Write a tiny original program that uses the concept.
6. Explain what you built in plain English.
7. Revisit the same topic a few days later from memory.

That last step is easy to skip and extremely important. If you only practice while the idea is fresh, you are testing short-term familiarity. When you return later and have to reconstruct the idea, you build durable memory.

One useful rule is: if you cannot write a small version of the concept from a blank file, you do not own it yet. You may understand it while reading, but you do not own it. Ownership means you can produce it.

## How To Move From Syntax To Judgment

Early learning is mostly about "what does this do?" Mid-level work is much more about "should I do it this way here?" That shift is subtle, and it is the main bridge this tutorial should help you cross.

For example, it is one thing to know that inheritance exists. It is another thing to choose an interface and composition because the app needs testable behavior, not a type hierarchy. It is one thing to know how to catch exceptions. It is another thing to decide that invalid user input should use a `Try` pattern or validation result, while a corrupted application state should throw. It is one thing to know how to write `async` methods. It is another thing to avoid blocking calls, pass cancellation tokens, and understand which operations are actually I/O-bound.

When studying each example, ask one extra question: "When would I not use this?" That question builds judgment.

Do not only ask:

- How do I write a `Dictionary`?
- How do I write a generic method?
- How do I call an API?
- How do I register a service?

Also ask:

- When is a `List<T>` better than a `Dictionary<TKey, TValue>`?
- When is a generic method unnecessary?
- When should HTTP failure become an exception versus a result?
- When should a service be scoped instead of singleton?

That second set of questions is where mid-level thinking begins.

## How To Practice With The Capstone

Chapter 22 is the most important chapter because it asks you to combine the pieces. Do not rush it. Build it once by following the chapter. Then build it again with changes.

Good second-pass changes include:

- Add users and assign tasks to users.
- Add due-date filtering and sorting.
- Add soft delete instead of hard delete.
- Add an audit field such as `CreatedBy` or `UpdatedAt`.
- Add validation beyond data annotations.
- Add paging metadata to list responses.
- Add a search endpoint.
- Add tests for failure cases, not just happy paths.
- Replace SQLite with SQL Server or PostgreSQL.
- Add authentication and protect write endpoints.

These changes force you to move through the layers: entity, DTO, DbContext, migration, service, controller, tests. That is the real shape of backend C# work. Most job tasks are not isolated syntax tasks. They are cross-layer edits.

When you make a capstone change, write down the files you touched and why. That habit mirrors professional pull request descriptions. It also trains you to think in terms of impact, not just code.

## Gaps To Expand Next

The tutorial is a strong foundation for CRUD/backend work, but there are still gaps to close before you can confidently contribute at a mid-level on a real team.

The first gap is Git workflow. You need to be comfortable with branches, commits, pull requests, reviews, merge conflicts, rebasing, reverting, and reading diffs. This should become normal muscle memory. A developer who writes decent code but cannot manage their changes cleanly slows the team down.

The second gap is SQL beyond EF Core. EF Core is useful, but you still need to understand joins, indexes, query plans, transactions, constraints, and why some queries are slow. Mid-level backend developers are often expected to diagnose database problems, not just write LINQ.

The third gap is authentication and authorization. Learn the difference between who a user is and what they are allowed to do. Learn JWTs, OAuth/OIDC at a conceptual level, ASP.NET Core authorization policies, role and claim checks, password hashing, secret storage, and the habit of never trusting client input.

The fourth gap is deployment. You should know how to run your app outside your IDE. Learn environment variables, appsettings per environment, Docker basics, CI builds, test pipelines, and the idea of deploying to staging before production. You do not need to become a platform engineer, but you should understand the path from code to running service.

The fifth gap is observability. Logging is the start, not the end. Learn structured logs, correlation IDs, metrics, traces, health checks, and how to answer the production question: "What is the app doing right now, and why did this request fail?"

The sixth gap is security mindset. This includes input validation, output encoding, SQL injection, over-posting, protecting secrets, least privilege, safe error messages, dependency updates, and avoiding sensitive data in logs. Security is not a chapter you finish. It is a way of thinking while you code.

The seventh gap is performance. Learn to measure before optimizing. Practice with profilers, BenchmarkDotNet, EF Core query logging, `AsNoTracking`, pagination, streaming, caching, and avoiding accidental large allocations. Mid-level developers should be able to make code faster without guessing wildly.

The eighth gap is working in existing code. Tutorials start from clean files. Jobs usually do not. Practice reading unfamiliar repositories. Trace a request from controller to database. Find where a service is registered. Follow a failing test. Change a small behavior without rewriting the whole system. This is one of the most job-realistic skills you can build.

The ninth gap is communication. Learn to write clear commit messages, PR descriptions, bug reports, and technical notes. Learn to ask questions that include what you tried, what happened, what you expected, and where you are stuck. A mid-level developer is not someone who never needs help. It is someone who can make progress visible and ask for help efficiently.

## A Suggested Roadmap After This Tutorial

After finishing the tutorial once, do not immediately hunt for another beginner course. Build.

First, rebuild the chapter 22 API from memory. Let yourself look things up, but do not follow the chapter line by line. The goal is retrieval.

Second, add three features to it. Pick one data feature, one API behavior feature, and one test feature. For example: add task labels, add filtering by label, and add integration tests for the filter.

Third, deploy it somewhere. Even a simple deployment teaches configuration, connection strings, logging, environment differences, and the humbling fact that "works locally" is only the first gate.

Fourth, put it on GitHub with a clean README. Include what it does, how to run it, how to run tests, what tradeoffs you made, and what you would improve next. This turns the project into evidence of how you think.

Fifth, read a real C# codebase. Do not try to understand all of it. Pick one feature path and trace it. Write notes. Look at naming, layering, tests, error handling, and dependency injection. You are training yourself to enter an unfamiliar codebase without needing someone to narrate every file.

Sixth, do one small contribution-style task. It can be in your own project: open an issue, create a branch, make the change, write tests, open a pull request, review your own diff, then merge. The workflow matters.

## What Mid-Level Contribution Looks Like

A mid-level C# developer is not just someone who knows more syntax than a junior. A mid-level developer can take a reasonably scoped problem and move it toward production with limited hand-holding.

That usually means they can clarify requirements, identify the files likely to change, make a small design choice, implement it, test it, handle common failure cases, and explain the tradeoffs. They can read compiler errors and runtime exceptions without spiraling. They can debug. They can ask for review on a focused change. They can receive feedback without defensiveness. They can notice when a task is bigger than it first appeared and communicate that early.

This tutorial gives you many of the raw materials for that. The companion examples help make the raw materials familiar. The capstone starts combining them. But the final step is repeated practice under more realistic constraints.

So the goal is not to finish these files and declare yourself done. The goal is to use these files until the basic moves are no longer consuming all your attention. Once syntax feels ordinary, your mind has room for the real work: correctness, design, maintainability, performance, security, and communication.

That is the path from tutorial learner to useful teammate.

---

Back to the [README index](./README.md).
