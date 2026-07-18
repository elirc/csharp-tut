# 00 - Setup and Tooling Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `node --version` | Shows the installed Node.js version. |
| 2 | `npm --version` | Shows the installed npm version. |
| 3 | `mkdir app && cd app` | Creates and enters a project folder. |
| 4 | `npm init -y` | Creates a default `package.json`. |
| 5 | `"type": "module"` | Enables ES module syntax in Node.js. |
| 6 | `"start": "node src/index.js"` | Adds a runnable npm script. |
| 7 | `npm start` | Runs the project's `start` script. |
| 8 | `node src/index.js` | Runs a file directly with Node. |
| 9 | `npm install lodash` | Adds a runtime dependency. |
| 10 | `npm install -D vitest` | Adds a development-only dependency. |
| 11 | `npm uninstall lodash` | Removes a dependency. |
| 12 | `npm list --depth=0` | Lists top-level installed packages. |
| 13 | `npm outdated` | Checks for package updates. |
| 14 | `package-lock.json` | Locks exact dependency versions. |
| 15 | `node_modules/` | Stores installed package files locally. |
| 16 | `npm install` | Restores packages from `package.json`. |
| 17 | `npm run test` | Runs a custom script named `test`. |
| 18 | `console.log(process.cwd())` | Prints the current working directory in Node. |
| 19 | `process.argv` | Reads command-line arguments. |
| 20 | `.gitignore` with `node_modules/` | Prevents generated dependencies from being committed. |
