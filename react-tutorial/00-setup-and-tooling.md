# 00 - Setup and Tooling

Modern React projects usually use a build tool. The React docs recommend starting from a build tool such as Vite, Parcel, or Rsbuild when building from scratch. Vite is a common default for learning because it is fast and simple.

```bash
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

For TypeScript:

```bash
npm create vite@latest my-react-app -- --template react-ts
```

The important starter files are usually:

- `src/main.jsx` or `src/main.tsx`: mounts the React app.
- `src/App.jsx` or `src/App.tsx`: the starter component.
- `index.html`: the HTML shell where React attaches.
- `package.json`: scripts and dependencies.

React development also depends on browser dev tools, React Developer Tools, npm scripts, formatting, and your editor's TypeScript/JavaScript support.

## Exercises

1. Create a React app with Vite.
2. Run the dev server.
3. Change `App.jsx` and watch the browser update.
4. Add a new component file and import it.
5. Build the production bundle with `npm run build`.
