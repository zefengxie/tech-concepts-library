# Bundler

## 1. Definition
A **bundler** is a build tool that takes your project’s source files (JavaScript, CSS, images, etc.) and **combines, transforms, and optimizes** them into one or more output files (bundles) that can be efficiently loaded by the browser.

Common bundlers:
- Webpack
- Vite (uses Rollup/esbuild under the hood)
- Parcel
- Rollup
- esbuild

---

## 2. Why Bundlers Are Important

- **Fewer HTTP requests**: bundle many modules into fewer files.
- **Modern syntax support**: transpile ES6+ to browser-compatible JS.
- **Optimization**: minification, tree shaking, code splitting, caching.
- **Module system**: let you write modular code with `import` / `export`.
- **Asset pipeline**: handle CSS, images, fonts, etc. as first-class imports.

---

## 3. Good Usage Example

```bash
# Development
npm run dev

# Production build
npm run build
```

Example `webpack.config.js` (simplified):

```js
module.exports = {
  entry: "./src/index.tsx",
  output: {
    filename: "bundle.js",
    path: __dirname + "/dist",
  },
  module: {
    rules: [
      { test: /\.tsx?$/, loader: "ts-loader" },
      { test: /\.css$/, use: ["style-loader", "css-loader"] },
    ],
  },
};
```

---

## 4. Bad Usage / Anti-patterns

- Treating the bundler as a **black box** and never understanding basic config.
- Huge single bundle with no code splitting → slow initial load.
- Including large libraries globally when only used in one small feature.
- Not using production mode (no minification, no tree shaking).

---

## 5. Interview Questions

- What is a bundler and why do we need one?
- Why don’t we just load many script tags without a bundler?
- What are typical optimizations a bundler can perform?
- Compare Webpack, Vite, and Parcel at a high level.
- How does bundling relate to tree shaking and code splitting?

---

## 6. Quick Summary

- Bundler = tool that takes your modular source code and creates optimized bundles for the browser.
- Enables modern JS features, optimizations, and better DX.
- Often combined with dev server, HMR, and other tooling.
