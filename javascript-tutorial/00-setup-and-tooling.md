# 00 - Setup and Tooling

JavaScript can run in the browser and in runtimes such as Node.js. For real development, you need both a runtime and a package manager. Node.js lets you run JavaScript from the terminal, build backend services, run tests, and use modern tooling.

Install the current LTS version of Node.js. Then check that `node` and `npm` are available:

```bash
node --version
npm --version
```

A basic project usually starts with a folder and a `package.json` file:

```bash
mkdir hello-js
cd hello-js
npm init -y
```

The `package.json` file describes your project, scripts, dependencies, and module settings. For modern JavaScript, many projects use ES modules:

```json
{
  "type": "module",
  "scripts": {
    "start": "node src/index.js"
  }
}
```

Create a `src/index.js` file and run it with `npm start`.

Tooling matters early. Learn your editor's formatter, debugger, integrated terminal, and JavaScript language support. You will use them constantly. Also learn the difference between dependencies, dev dependencies, scripts, lock files, and the `node_modules` folder.

## Exercises

1. Create a new Node project.
2. Add a `start` script.
3. Create `src/index.js` and print a message.
4. Install a small package and import it.
5. Delete `node_modules`, run `npm install`, and observe what comes back.
