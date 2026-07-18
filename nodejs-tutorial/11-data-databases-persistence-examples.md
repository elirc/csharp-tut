# 11 - Data, Databases, and Persistence Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const tasks = []` | In-memory storage. |
| 2 | `JSON.stringify(data, null, 2)` | Serialize data for file storage. |
| 3 | `JSON.parse(text)` | Parse stored JSON. |
| 4 | `crypto.randomUUID()` | Creates record ids. |
| 5 | `createdAt` | Creation timestamp field. |
| 6 | `updatedAt` | Update timestamp field. |
| 7 | `deletedAt` | Soft-delete timestamp field. |
| 8 | `pg` | Popular PostgreSQL driver. |
| 9 | `mysql2` | Popular MySQL driver. |
| 10 | `better-sqlite3` | SQLite package option. |
| 11 | `Prisma` | Popular ORM. |
| 12 | `Drizzle` | TypeScript-friendly ORM/query builder. |
| 13 | `Mongoose` | MongoDB object modeling library. |
| 14 | `Redis` | Cache/data structure server. |
| 15 | `migration` | Database schema change. |
| 16 | `transaction` | Atomic group of operations. |
| 17 | `connection pool` | Reuses database connections. |
| 18 | `parameterized query` | Protects against SQL injection. |
| 19 | `N plus one` | Repeated query performance bug. |
| 20 | `repository boundary` | Keeps persistence details contained. |
| 21 | `SELECT ... WHERE id = $1` | Parameterized PostgreSQL-style query. |
| 22 | `pool.query(sql, params)` | Executes a query through a connection pool. |
| 23 | `BEGIN / COMMIT / ROLLBACK` | Transaction control statements. |
| 24 | `unique constraint` | Database rule preventing duplicate values. |
| 25 | `foreign key` | Database relationship constraint. |
| 26 | `index` | Data structure that speeds lookups. |
| 27 | `pagination` | Limits result size for list endpoints. |
| 28 | `cursor pagination` | Uses stable cursor rather than offset. |
| 29 | `seed data` | Initial data for local development or tests. |
| 30 | `migration rollback` | Reverts a schema change when supported. |

## Interview Questions

**Q1. Why is in-memory storage not enough for production APIs?**  
Data disappears on restart and is not shared across multiple processes or machines.

**Q2. What is a database migration?**  
A controlled change to database schema, such as creating tables, adding columns, or changing indexes.

**Q3. Why use parameterized queries?**  
They separate SQL structure from user input and help prevent SQL injection.

**Q4. What is a connection pool?**  
It reuses database connections instead of opening a new connection for every query.

**Q5. What is the N+1 query problem?**  
It happens when code loads one set of records and then performs one extra query per record, causing poor performance.
