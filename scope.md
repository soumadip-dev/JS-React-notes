### JavaScript Pillars

JavaScript has four main pillars:

1. **Scopes**
2. **Coercion and Types**
3. **Async Programming**
4. **Objects and Classes**

#### Scopes in JavaScript
- Scopes determine where variables or functions are accessible in the code.
- JavaScript's scoping mechanism is different from other languages like Java, C++, and Python, so avoid mixing these concepts.

---

**Note:** To fully understand scoping in JavaScript, it's essential to grasp the difference between compiled and interpreted languages:

- **Compiled Languages (e.g., C, C++):** Code is analyzed and compiled into an executable binary by a compiler. Errors must be fixed before any code is executed.
- **Interpreted Languages (e.g., Bash):** Code is executed line by line. If an error occurs, execution stops at the error.
- **Hybrid Languages (e.g., Java, JavaScript, Python):** These languages use both compilation and interpretation for code execution.

---

##### Nature of JavaScript

JavaScript combines the processes of compilation and interpretation. Many people think it is interpreted only, but that's not true. Here's why:

```javascript
console.log("wow wow"); 
function fun() { 
  let r = uy; 
}
```

The above code throws an error without executing `console.log` and prints nothing on the console, simply displaying the error. This shows that JavaScript analyzes the code beforehand before starting execution.

##### Phases of Execution in JavaScript
JavaScript executes code in two distinct phases:
1. **Compilation and Scope Resolution Phase:** During this phase, JavaScript determines the scope and visibility of each variable and function. This is known as **scope resolution**.
2. **Interpretation or Execution Phase:** In this phase, the actual code execution takes place, with values assigned to the variables within the determined scopes.

##### Types of Scope in JavaScript

1. **Global Scope**
2. **Function Scope**
3. **Block Scope**

##### 1. Global Scope

Variables declared outside any function or block are in the global scope. These variables are accessible from anywhere in your code, whether inside a function, loop, or conditional statement.

Example:
```javascript
let x = 10;

function showX() {
    console.log(x); // Accessible here
}

showX(); // Outputs: 10

console.log(x); // Accessible here too
```

In the above code, `x` is declared in the global scope, so it can be accessed both inside and outside the `showX` function.

##### 2. Function Scope

When a variable is declared inside a function, it is scoped to that function, meaning it can only be accessed within that function.

Example:
```javascript
function showY() {
    let y = 20;
    console.log(y); // Accessible here
}

showY(); // Outputs: 20

console.log(y); // Error: y is not defined
```

Here, `y` is declared inside the `showY` function, so it is only accessible within that function and not outside.

##### 3. Block Scope

Block scope is created when variables are declared inside a pair of curly braces `{}`. This can happen within loops, conditionals, or just standalone blocks.

Example:
```javascript
if (true) {
    let z = 30;
    console.log(z); // Accessible here
}

console.log(z); // Error: z is not defined
```

In this example, `z` is declared inside an `if` block, so it is only accessible within that block.

#### Variable Declarations and Scope

In JavaScript, variables can be declared using `var`, `let`, or `const`. The way a variable is declared affects its scope.

##### Any variable is used only in two ways:

- **RHS (Right-Hand Side):** When we consume the variable.
- **LHS (Left-Hand Side):** When we assign a value or declare the variable.

##### Lexical Scoping / Lexical Parsing:
JavaScript uses lexical scoping, also known as static scoping. In lexical scoping, scopes are allocated during compile time. So, in JavaScript, values for variables are assigned in the execution phase, but the scope of the variable is decided during the compilation phase.

Example:
```javascript
function outerFunction() {
    let outerVar = 'I am outside!';
    
    function innerFunction() {
        console.log(outerVar); // Accessible due to lexical scoping
    }
    
    innerFunction();
}

outerFunction(); // Outputs: I am outside!
```

In the above code, `innerFunction` can access `outerVar` because it is lexically within the scope of `outerFunction`.


##### **`var`:**
Variables declared with `var` are either function-scoped or globally scoped. `var` does not support block scope.

Example:
```javascript
function showVar() {
    if (true) {
        var a = 40;
    }
    console.log(a); // Accessible here, even though it's inside a block
}

showVar(); // Outputs: 40
```

**Another Example:**
```javascript
var x = 10;
console.log(x, y); // Output: 10 undefined

if (true) {
    var x = 20;
    var y = 30;
    console.log(x, y); // Output: 20 30
}

console.log(x, y); // Output: 20 30
```

In the above example, during the scope resolution phase, both `x` and `y` are declared in the global scope due to the use of `var`. When the execution phase starts:
- `x` has been initialized to `10`.
- `y` is declared in the global scope but has not been assigned a value yet, so it is `undefined`.

Within the `if` block, `x` and `y` are reassigned:
- `x` is updated to `20`.
- `y` is assigned the value `30`.

Since `var` has function or global scope, the changes to `x` and `y` in the `if` block affect their values globally. Thus, when the final `console.log(x, y);` is executed, both `x` and `y` have the values `20` and `30`, respectively.

---

| **Note:** _How is function scope different from block scope?_

A variable with function scope has a unique characteristic: it can be defined anywhere within the function but will still be accessible throughout the entire function.

```javascript
function fun() {
    console.log("The value of x here is", x); // The value of x here is undefined
    var x = 20;
    console.log("The value of x here is", x); // The value of x here is 20
}
```

If we try to do the same thing using `let` instead of `var`, it will result in an error because `let` does not have function scope like `var` does.


| **Note:** _Automatically Global:_
This refers to variables that are automatically added to the global scope under certain circumstances. Typically, this happens when you create a variable inside a function without using the `var`, `let`, or `const` keyword. These variables automatically become global variables, even though they are defined inside a function.

```javascript
var mentor = "Ram"; // 'mentor' is defined globally

function exampleFunction() {
    mentor = "Sam"; // This modifies the global 'mentor' variable
    language = "JavaScript"; // 'language' is not declared with var/let/const, so it becomes a global variable
    console.log("Learning", language, "with", mentor);
}

exampleFunction(); // Calls the function, modifying the global variables

console.log("Current mentor:", mentor); // Outputs the current value of 'mentor' -> Sam
console.log("Current language:", language); // Outputs the current value of 'language' -> JavaScript
```

- If we call `exampleFunction();` after the  
  `console.log("Current mentor:", mentor);` and  
  `console.log("Current language:", language);`, 
- The first `console.log` will show the mentor as `Ram`, because `exampleFunction()` has not yet been called to modify the global variable. 
- The second `console.log` will throw an error because the `language` variable is not yet defined globally until `exampleFunction();` is called.

Automatic global variables are problematic in JavaScript. To prevent this, you can enable **strict mode** using the following command at the top of your script: `"use strict";`.

---

##### `let` and **`const`**: 
These keywords support block scope, meaning variables declared inside a block are only accessible within that block.
  
  Example:
  ```javascript
  function showLetConst() {
      if (true) {
          let b = 50;
          const c = 60;
          console.log(b, c); // Accessible here
      }
      console.log(b, c); // Error: b and c are not defined
  }

  showLetConst();
  ```

###### Special Characteristics of `let` and `const`

- **Block Scope Inside Functions**: Variables declared with `let` or `const` are not hoisted in the same way as `var`. They are only accessible after their declaration, which differs from the function scope provided by `var`. This difference leads to the concept of the **Temporal Dead Zone (TDZ)**.

  Example:
  ```javascript
  function checkLet() {
      console.log(x); // Error: Cannot access 'x' before initialization
      let x = 10;
  }
  
  checkLet();
  ```

  Here, `x` is in the TDZ from the start of the block until the declaration is encountered, making it inaccessible before its declaration.

- **Temporal Dead Zone (TDZ)**: This is the region of the block before the variable is declared. A variable declared with `let`, `const`, or `class` is in the TDZ from the start of the block until the code execution reaches its declaration.

- **No Redeclaration**: Variables declared with `let` and `const` cannot be redeclared in the same scope. This is enforced during the first phase (compilation phase) of the JavaScript execution process.

  Example:
  ```javascript
  let d = 70;
  let d = 80; // Error: Identifier 'd' has already been declared
  ```

### Hoisting

**Hoisting** is a term commonly used in the JavaScript community, although it is not officially defined in the ECMAScript specification. It is a consequence of JavaScript's scoping mechanism, which involves two main phases of code execution: the **Compilation and Scope Resolution Phase** and the **Interpretation or Execution Phase**. During the compilation phase, many variables are already identified, so when the code is executed, it seems as though JavaScript is aware of these variables even before their actual declaration. This phenomenon, where the interpreter appears to move the declarations of functions, variables, classes, or imports to the top of their scope before execution, is known as **Hoisting**.

Here’s an example of hoisting with `var`:

```javascript
function hoistVar() {
    console.log(a); // undefined (due to hoisting)
    var a = 90;
    console.log(a); // 90
}

hoistVar();
```
