# 10 - Data Access with EF Core Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `DbContext` | EF Core unit-of-work context. |
| 2 | `DbSet<TaskItem>` | Query/update set for an entity. |
| 3 | `AddDbContext<T>()` | Registers DbContext with DI. |
| 4 | `UseSqlite()` | Configures SQLite provider. |
| 5 | `UseSqlServer()` | Configures SQL Server provider. |
| 6 | `dotnet ef migrations add Name` | Creates a migration. |
| 7 | `dotnet ef database update` | Applies migrations. |
| 8 | `SaveChangesAsync()` | Persists tracked changes. |
| 9 | `AsNoTracking()` | Faster read-only query. |
| 10 | `Include()` | Eager-loads related data. |
| 11 | `Where()` | Filters query. |
| 12 | `Select()` | Projects query results. |
| 13 | `FirstOrDefaultAsync()` | Gets first row or null. |
| 14 | `SingleOrDefaultAsync()` | Expects zero or one row. |
| 15 | `AddAsync()` | Adds new entity. |
| 16 | `Remove()` | Deletes entity. |
| 17 | `HasKey()` | Configures primary key. |
| 18 | `HasIndex()` | Configures index. |
| 19 | `transaction` | Groups database changes atomically. |
| 20 | `N plus one` | Performance bug from repeated related queries. |
