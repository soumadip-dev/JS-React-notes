## What is a Package Manager?

A **package manager** is a tool that automates the process of installing, updating, configuring, and managing libraries or packages for a specific programming language or platform. These packages are pre-written pieces of code or tools that developers can use to add features to their projects without having to write everything from scratch.
**Examples**:
- **npm** (The default package manager for JavaScript and Node.js. Open-source developers use `npm` to share software and tools.)
- **pip** (Python Package Installer)
- **Yarn** (An alternative to `npm` for managing JavaScript libraries)

##### How to Initialize npm?

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

##### Why Do We Need a Bundler?

- **Code Splitting**: Breaks your app into smaller pieces that load only when needed, speeding up performance.
- **Dependency Management**: Combines all the required dependencies into a single bundle.
- **Optimization**: Minifies JavaScript, removes unused code (tree-shaking), and compresses assets to load faster.
- **Development Tools**: Provides features like hot module replacement (HMR) to improve the development process.
- **Efficient File Transfer**: Reduces the time taken to transfer files, making your app load faster.

##### Examples of Bundlers:

- Webpack
- Parcel
- Vite

##### Installation Commands for Parcel:

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

```jsx
const myElement = React.createElement('h1', {}, 'I do not use JSX!');
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(myElement);
```

---
## Is `JSX` mandatory for React?

`JSX` is a syntax extension for JavaScript that allows you to write HTML-like code within JavaScript, making it easier to create and visualize React components. These `JSX` elements are ultimately compiled into calls to `React.createElement(component, props, ...children)`, which generate React elements that are rendered to the DOM.

While `JSX` simplifies writing React components and improves code readability, it is **not mandatory**. You can create React components and elements directly using plain JavaScript by calling `React.createElement`. However, without `JSX`, the code can become less intuitive and harder to maintain. For these reasons, most developers prefer using `JSX` to write clean and expressive React code.

---

## Is `ES6` mandatory for React?

`ES6` is not mandatory for `React` but is highly recommendable. The latest projects created on React rely a lot on ES6. React uses ES6, and you should be familiar with some of the new features like: Classes, Arrow Functions, Variables(let, const).
ES6 stands for ECMAScript 6. ECMAScript was created to standardize JavaScript, and ES6 is the 6th version of ECMAScript, it was published in 2015.

---
## What is Babel and how does it work in React?

In React, we write JSX, which looks like HTML but is not valid JavaScript syntax. Since browsers cannot understand JSX directly, it needs to be converted (or transpiled) into regular JavaScript. This is where Babel comes in.

Babel is a JavaScript transpiler that converts JSX into browser-compatible JavaScript code. It works behind the scenes when we use a bundler (like Webpack) to create a React app. The Babel setup is included in the `node_modules` folder of the React app.

In addition to transpiling JSX, Babel also converts modern JavaScript (like ES6+) into an older version of JavaScript that works in older browsers. This ensures compatibility across various browser environments.

---

## What is Component Composition?

Component Composition is a way to combine small, reusable pieces of code (called components) to build bigger and more complex parts of an application. It’s like using building blocks to create a structure. Each block does its own job, and together they make the whole thing work.

---

## What is `<React.Fragment></React.Fragment>` and `<></>`?

`<React.Fragment></React.Fragment>` is a feature in React that allows you to return multiple elements from a React component by allowing you to group a list of children without adding extra nodes to the DOM.
`<></>` is the shorthand tag for `React.Fragment`. The only difference between them is that the shorthand version does not support the key attribute.

##### Example

```jsx
return (
        <React.Fragment>
            <Header />
            <Navigation />
            <Main />
            <Footer />
        </React.Fragment>
    );

return (
        <>
            <Header />
            <Navigation />
            <Main />
            <Footer />
        </>
    );
```

---
## Props

Props are data passed from a parent component to a child component. They let the child component use this data for rendering or logic.

**Example:**

```jsx
// Parent Component
function App() {
  return <Greeting name="Soumadip" />;
}

// Child Component
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}
```

**Output:**  
Hello, Soumadip!

---
## What is `Config Driven UI`?

A `Config Driven UI` is built based on the configuration data that the application receives. This approach makes the app more dynamic and easier to update. It is a common and simple way to create user interfaces that can adapt to changes.

Using a `Config Driven UI` saves development time and effort, as it provides a flexible and reusable structure. For example, a login form in most apps often requires updates, such as adding new form validations, dropdown options, or design changes. With a `Config Driven UI`, these updates can be handled easily without rewriting a lot of code.

---

## Why do we need `keys` in React?

A `key` is a special attribute you need to include when creating lists of elements in React. Keys are used in React to identify which items in the list are changed, updated, or deleted. In other words, we can say that keys are unique Identifier used to give an identity to the elements in the lists.
Keys should be given to the elements within the array to give the elements a stable identity.
##### Example

```
<li key={0}>1</li>
<li key={1}>2</li>
<li key={2}>3</li>
```

---

## Can we use `index as keys` in React?

Yes, we can use the `index as keys`, but it is not considered as a good practice to use them because if the order of items may change. This can negatively impact performance and may cause issues with component state.
Keys are taken from each object which is being rendered. There might be a possibility that if we modify the incoming data react may render them in unusual order.

---

## What is the difference between `Named export`, `Default export`, and `* as export`?

In ES6, we can export and import modules to use them in other files. There are three main ways to export modules: `Named export`, `Default export`, and `* as export`.

##### 1. Named Export

- Allows multiple named exports per file.
- When importing, the names must match exactly, and `{}` braces are required.

**Example:**  
Exporting components from `MyComponent.js`:

```jsx
export const MyComponent = () => {};
export const MyComponent2 = () => {};
```

Importing named exports:

```jsx
// Importing a single named export
import { MyComponent } from "./MyComponent";

// Importing multiple named exports
import { MyComponent, MyComponent2 } from "./MyComponent";

// Renaming an import
import { MyComponent2 as MyNewComponent } from "./MyComponent";
```

##### 2. Default Export

- Only one default export is allowed per file.
- The import name can be anything, and `{}` braces are not used.

**Example:**  
Exporting a default component from `MyComponent.js`:

```jsx
const MyComponent = () => {};
export default MyComponent;
```

Importing the default export:

```jsx
import MyComponent from "./MyComponent";
```

##### 3. * as Export

- Imports all the exports from a module as a single object, which allows accessing individual exports as properties.

**Example:**  
Exporting multiple components from `MyComponent.js`:

```jsx
export const MyComponent = () => {};
export const MyComponent2 = () => {};
export const MyComponent3 = () => {};
```

Importing all exports:

```jsx
import * as MainComponents from "./MyComponent";
```

Using in JSX:

```jsx
<MainComponents.MyComponent />
<MainComponents.MyComponent2 />
<MainComponents.MyComponent3 />
```

##### Combining `Named Export` and `Default Export`

You can use both export types in the same file.

**Example:**  
Exporting:

```jsx
export const MyComponent2 = () => {};
const MyComponent = () => {};
export default MyComponent;
```

Importing:

```jsx
import MyComponent, { MyComponent2 } from "./MyComponent";
```

This approach gives flexibility by allowing both named and default imports.

---
---
---
