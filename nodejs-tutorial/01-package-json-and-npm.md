# 01 - Package JSON and npm

`package.json` is the manifest for a Node project. It stores project metadata, scripts, dependencies, package type, entry points, and tool configuration.

Create one:

```bash
npm init -y
```

Example:

```json
{
  "type": "module",
  "scripts": {
    "start": "node src/index.js",
    "test": "node --test"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

Runtime dependencies are needed by the app. Dev dependencies are needed while developing, testing, or building.

The lock file records exact resolved package versions. Commit it for applications.

## Exercises

1. Create `package.json`.
2. Add `start` and `test` scripts.
3. Install a runtime dependency.
4. Install a dev dependency.
5. Remove a dependency and inspect the manifest.
