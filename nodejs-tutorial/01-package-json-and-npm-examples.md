# 01 - Package JSON and npm Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `npm init -y` | Creates default `package.json`. |
| 2 | `npm install express` | Adds runtime dependency. |
| 3 | `npm install -D vitest` | Adds development dependency. |
| 4 | `npm uninstall express` | Removes dependency. |
| 5 | `npm install` | Restores dependencies. |
| 6 | `npm ci` | Clean install for CI from lock file. |
| 7 | `npm run start` | Runs a package script. |
| 8 | `npm test` | Runs the `test` script. |
| 9 | `dependencies` | Runtime packages. |
| 10 | `devDependencies` | Development packages. |
| 11 | `package-lock.json` | Exact dependency resolution lock. |
| 12 | `node_modules` | Installed package folder. |
| 13 | `"type": "module"` | Treats `.js` files as ES modules. |
| 14 | `"main": "index.js"` | Common package entry point field. |
| 15 | `"exports"` | Modern package entry point map. |
| 16 | `"engines"` | Declares supported Node/npm versions. |
| 17 | `npm outdated` | Checks for available updates. |
| 18 | `npm audit` | Checks known dependency vulnerabilities. |
| 19 | `npx command` | Runs a package binary. |
| 20 | `semantic versioning` | Version pattern used by packages. |
| 21 | `npm pkg set type=module` | Updates `package.json` from the CLI. |
| 22 | `npm pkg get scripts` | Prints the scripts section from `package.json`. |
| 23 | `npm update` | Updates packages within allowed semver ranges. |
| 24 | `npm install package@1.2.3` | Installs a specific package version. |
| 25 | `npm view express version` | Checks the latest published package version. |
| 26 | `npm fund` | Shows funding information for dependencies. |
| 27 | `npm explain package-name` | Explains why a package is installed. |
| 28 | `npm dedupe` | Attempts to reduce duplicate dependency copies. |
| 29 | `"private": true` | Prevents accidentally publishing an app package. |
| 30 | `"scripts": { "dev": "node --watch src/server.js" }` | Creates a development script for automatic restarts. |

## Interview Questions

**Q1. What is `package.json` used for?**  
It describes a Node project: metadata, scripts, dependencies, module type, package entry points, and tooling configuration.

**Q2. What is the difference between `dependencies` and `devDependencies`?**  
`dependencies` are needed at runtime. `devDependencies` are used for development tasks such as testing, linting, building, or formatting.

**Q3. Why should applications commit `package-lock.json`?**  
It records exact dependency resolutions, making installs more reproducible across machines and CI.

**Q4. What is the difference between `npm install` and `npm ci`?**  
`npm install` can update the lock file. `npm ci` performs a clean install from the lock file and is preferred in CI.

**Q5. What does semantic versioning communicate?**  
It communicates compatibility expectations through major, minor, and patch versions: breaking changes, features, and fixes.
