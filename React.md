## What is a Package Manager?

A **package manager** is a tool that automates the process of installing, updating, configuring, and managing libraries or packages for a specific programming language or platform. These packages are pre-written pieces of code or tools that developers can use to add features to their projects without having to write everything from scratch.
**Examples**:
- **npm** (The default package manager for JavaScript and Node.js. Open-source developers use `npm` to share software and tools.)
- **pip** (Python Package Installer)
- **Yarn** (An alternative to `npm` for managing JavaScript libraries)

#### How to Initialize npm?

To initialize `npm`, you can use the following commands:

```bash
npm init
```

To skip the setup step and allow `npm` to create the `package.json` file automatically without configurations, use:

```bash
npm init -y
```

---
## Bundlers in React: A Simple Overview

- A bundler combines your JavaScript, CSS, and other assets into one or a few files.
- This reduces the number of files the browser needs to load, making your application load faster.
- During development, your code remains modular, but the bundler ensures efficient delivery in production.
- By reducing the number of files, bundlers improve the efficiency of file transfers from the server to the client.
- This process helps in enhancing load times and overall performance of the application.

#### Why Do We Need a Bundler?

- **Code Splitting**: Breaks your app into smaller pieces that load only when needed, speeding up performance.
- **Dependency Management**: Combines all the required dependencies into a single bundle.
- **Optimization**: Minifies JavaScript, removes unused code (tree-shaking), and compresses assets to load faster.
- **Development Tools**: Provides features like hot module replacement (HMR) to improve the development process.
- **Efficient File Transfer**: Reduces the time taken to transfer files, making your app load faster.

#### Examples of Bundlers:

- Webpack
- Parcel
- Vite

#### Installation Commands for Parcel:

- **Install:**
    
    ```
    npm install -D parcel
    ```
    
    `-D` is used for development and as a development dependency.
    
- **Parcel Commands:**
    
    - For development build:
        
        ```
        npx parcel <entry_point>
        ```
        
    - For production build:
        
        ```
        npx parcel build <entry_point>
        ```
        
---
## What is the `.parcel-cache` folder?

The `.parcel-cache` folder (or `cache folder` in Parcel v2) stores information about your project during the build process. It helps Parcel avoid re-parsing and re-analyzing everything from scratch during rebuilds, making it faster, especially in development mode.

---
## What is `npx`?

`npx` stands for `Node Package eXecute`. It’s a tool that allows you to run any JavaScript package from the npm registry without needing to install it. `npx` comes pre-installed with npm version 5.2 and later.

---
## What is the difference between `dependencies` and `devDependencies`?

- **`dependencies`**: Packages needed for your application to run in production.
- **`devDependencies`**: Packages required only for development and testing.

---
## What is Tree Shaking in Parcel?

Tree shaking (or dead code elimination) removes unused code from your production build. By analyzing your source code, Parcel can exclude parts that aren’t being used, making your final bundle smaller and faster to load.

---
## What is Hot Module Replacement?

Hot Module Replacement (HMR) allows you to replace, add, or remove modules in your application while it’s running, without a full page reload. This helps speed up development by keeping the app state intact and only updating the changed parts.

---
## What is the difference between `package.json` and `package-lock.json` files?

- `package.json` defines project properties, such as dependencies, scripts, description, author, license, and more.
- `package-lock.json` locks dependencies to specific version numbers, ensuring consistent installations across environments.
- `package-lock.json` tracks the entire tree of dependencies (including dependencies of dependencies) and the exact version of each.
- `package-lock.json` is a generated file and is not designed to be manually edited.
- You should commit `package-lock.json` to your code repository.
- `package-lock.json` cannot be published and is ignored if found outside the root of the project.
- Avoid manually updating `package.json` to prevent breaking synchronization with `package-lock.json`.

---
## What is `node_modules`? Is it a good idea to push that on git?

The `node_modules` folder contains generated code. This is not code you've written and you should never make any updates to the files inside Node modules as it may be overwritten when installing modules.
It’s better to not commit the `node_modules` folder and add it to `.gitignore`.
Reasons not to commit:
- The folder can be very large (up to Gigabytes).
- It can be easily recreated using `package.json`.

---
## What is the `dist` folder?

The `dist` folder stands for "distributable." It contains the minified (compressed) version of your code. This is the code that is used in live, production websites.

In Parcel, the default folder for your output files is called `dist`. If you want to change the name, you can use the `--dist-dir public` tag to make the folder called `public` instead of `dist` to avoid confusion.

---
## What is `browserlists`?

**Browserslist** is a tool used to specify which browsers your project should support. It helps you define a list of supported browsers (like Chrome, Firefox, Safari, etc.) based on certain criteria, such as the most popular browsers or a specific version range.

---

## What is `^` (caret) and `~` (tilde) in `package.json`?

These are version specifiers used to define dependency versions in `package.json`:

- **`~` (Tilde):** Allows patch updates only. For example, `~1.2.3` allows versions like `1.2.4`, `1.2.5`, but not `1.3.0`.
- **`^` (Caret):** Allows minor and patch updates. For example, `^1.2.3` allows versions like `1.3.0`, `1.4.0`, but not `2.0.0`.

**Key Difference:**  
`~` is stricter, allowing only patch updates, while `^` is more flexible, permitting minor and patch updates.

---

## What are Script Types in HTML?

The `<script>` tag’s `type` attribute specifies the type of script:

1. **`type="text/javascript"` (default):** Specifies regular JavaScript. This is the default type and can be omitted.
    
    ```html
    <script type="text/javascript">
      console.log("Hello!");
    </script>
    ```
    
2. **`type="module"`:** Indicates the script is a JavaScript module, enabling ES6 features like `import` and `export`. Modules automatically run in strict mode and have module-level scope.
    
    ```html
    <script type="module">
      import { sayHello } from './module.js';
      sayHello();
    </script>
    ```
    
3. **`type="application/json"`:** Embeds JSON data into a webpage, which can be fetched or parsed with JavaScript.
    
    ```html
    <script type="application/json" id="config">
      { "theme": "dark", "lang": "en" }
    </script>
    ```
    
4. **`type="text/plain"`:** Prevents the script from being executed. Often used for embedding raw content or templates.
    
    ```html
    <script type="text/plain" id="template">
      <h1>Welcome!</h1>
    </script>
    ```

5. **`type="text/babel"`:** Indicates that the script is written using Babel, a JavaScript transpiler. Babel is required to transpile and run this script type.
    ```html
    <script type="text/babel">
      const App = () => <h1>Hello, JSX!</h1>;
    </script>
    ```

6. **`type="text/typescript"`:** Ipecifies that the script is written in TypeScript. The browser cannot execute TypeScript directly; it must be transpiled to JavaScript using a tool like the TypeScript compiler or Babel.
    ```html
    <script type="text/typescript">
      let message: string = "Hello, TypeScript!";
      console.log(message);
    </script>
    ```

**Note:** If the `type` attribute is omitted, the default script type is assumed to be JavaScript.

---
## What is `JSX`?

- JSX stands for **JavaScript XML**.
- JSX allows you to write HTML elements directly within JavaScript and insert them into the DOM without using `createElement()` or `appendChild()` methods.
- JSX simplifies the process of writing and adding HTML in React applications.
- JSX converts HTML tags into React elements.
- With JSX, you can write both the markup and logic of a component in a single `.jsx` file, making it easier to maintain and debug.

-  **Example 1: Using JSX**

```jsx
const myElement = <h1>I Love JSX!</h1>; // use () in multiline JSX
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(myElement);
```

- **Example 2: Without JSX**

```javascript
const myElement = React.createElement('h1', {}, 'I do not use JSX!');
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(myElement);
```

---

## What is Babel and how does it work in React?

In React, we write JSX, which looks like HTML but is not valid JavaScript syntax. Since browsers cannot understand JSX directly, it needs to be converted (or transpiled) into regular JavaScript. This is where Babel comes in.

Babel is a JavaScript transpiler that converts JSX into browser-compatible JavaScript code. It works behind the scenes when we use a bundler (like Webpack) to create a React app. The Babel setup is included in the `node_modules` folder of the React app.

In addition to transpiling JSX, Babel also converts modern JavaScript (like ES6+) into an older version of JavaScript that works in older browsers. This ensures compatibility across various browser environments.

---
