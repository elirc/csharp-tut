# 15 - Security and Secrets

Security is not one feature. It affects configuration, logging, validation, data access, authentication, authorization, deployment, and dependency management.

Core habits:

- Do not commit secrets.
- Do not log secrets or sensitive personal data.
- Validate input.
- Use parameterized queries or safe ORM patterns.
- Use authentication to identify users.
- Use authorization to decide what users can do.
- Keep packages updated.
- Use HTTPS.
- Prefer least privilege.

ASP.NET Core provides authentication and authorization middleware:

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

Local development secrets can use user secrets. Production secrets should come from secure providers such as environment variables, cloud secret stores, or vaults.

## Exercises

1. Store a local secret with user secrets.
2. Add an authorization attribute to an endpoint.
3. Remove sensitive data from logs.
4. Validate request input.
5. Check packages for vulnerabilities.
