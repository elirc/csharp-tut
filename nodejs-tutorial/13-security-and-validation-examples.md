# 13 - Security and Validation Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `typeof title === "string"` | Runtime input check. |
| 2 | `title.trim() !== ""` | Rejects blank text. |
| 3 | `Number.isInteger(id)` | Validates integer value. |
| 4 | `Array.isArray(value)` | Validates array shape. |
| 5 | `Zod` | Popular validation library. |
| 6 | `safeParse` | Validation success/failure pattern. |
| 7 | `helmet` | Express security headers middleware. |
| 8 | `cors` | Cross-origin request configuration. |
| 9 | `rate limit` | Limits repeated requests. |
| 10 | `bcrypt` | Password hashing library family. |
| 11 | `never store plaintext passwords` | Critical security rule. |
| 12 | `JWT` | Common token format. |
| 13 | `OAuth/OIDC` | Common identity protocols. |
| 14 | `authorization check` | Verifies user can perform action. |
| 15 | `parameterized query` | Prevents SQL injection. |
| 16 | `escape output` | Prevents injection in rendered content. |
| 17 | `npm audit` | Checks dependency advisories. |
| 18 | `secret scanning` | Detects leaked credentials. |
| 19 | `process.env.SECRET` | Reads secret from environment. |
| 20 | `least privilege` | Grant only required permissions. |
| 21 | `express-rate-limit` | Common Express rate-limiting middleware. |
| 22 | `httpOnly cookie` | Cookie inaccessible to browser JavaScript. |
| 23 | `SameSite cookie` | Helps reduce cross-site request risks. |
| 24 | `CSRF token` | Protects state-changing browser requests. |
| 25 | `input allowlist` | Accepts only known-good values. |
| 26 | `output encoding` | Prevents user content from becoming executable markup. |
| 27 | `dependency pinning` | Reduces surprise changes from packages. |
| 28 | `npm audit fix` | Attempts to update vulnerable dependencies. |
| 29 | `refresh token rotation` | Reduces risk from stolen refresh tokens. |
| 30 | `security headers` | Browser-facing headers that reduce attack surface. |

## Interview Questions

**Q1. What is the difference between authentication and authorization?**  
Authentication identifies who the user is. Authorization decides what that user is allowed to do.

**Q2. Why should API input be validated at runtime?**  
Clients can send malformed, missing, malicious, or unexpected data regardless of frontend checks.

**Q3. What is SQL injection?**  
It is an attack where user input is interpreted as SQL code. Parameterized queries help prevent it.

**Q4. Why should passwords not be stored as plaintext?**  
If the database leaks, plaintext passwords immediately compromise users. Passwords should be salted and hashed with a suitable algorithm.

**Q5. Why avoid logging secrets?**  
Logs are often copied to external systems and viewed by many people, so secrets in logs can become long-lived credentials leaks.
