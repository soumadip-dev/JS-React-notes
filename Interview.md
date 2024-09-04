# INTERVIEW QUESTIONS

### What is `Object.seal()` in JavaScript?

`Object.seal()` is a method in JavaScript that seals an object, preventing the addition or deletion of properties while still allowing the modification of existing properties' values. This is useful when you want to keep the object's structure the same but still allow updates to its data.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.seal(product);

product.company = "Apple"; // Not allowed, new properties cannot be added
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed, properties cannot be deleted
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max"; // Existing properties can be modified
console.log(product); // { name: "iPhone 14 Pro Max", price: 125000 }
```

### What is `Object.freeze()` in JavaScript?

`Object.freeze()` is a method in JavaScript that freezes an object, making it immutable. This means that no properties can be added, deleted, or modified. The object's structure and data remain exactly as they were when the object was frozen. This is useful when you want to prevent any changes to an object.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.freeze(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }
```

### What is `Object.preventExtensions()` in JavaScript?

`Object.preventExtensions()` is a method in JavaScript that prevents the addition of new properties to an object but allows the deletion and modification of existing properties' values.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.preventExtensions(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Allowed
console.log(product); // { name: "iPhone 14 Pro" }

product.name = "iPhone 14 Pro Max"; // Allowed
console.log(product); // { name: "iPhone 14 Pro Max" }
```

### What are `Object.isFrozen()` and `Object.isSealed()`?

These methods check if an object is frozen or sealed.

```javascript
console.log(Object.isFrozen(product)); // true or false
console.log(Object.isSealed(product)); // true or false
```

### What is the difference between `let`, `var`, and `const`?

- `let` and `const` were introduced in ES6, while `var` is from older versions of JavaScript.
- Variables declared with `var` are function-scoped, meaning they are accessible throughout the entire function.
- `let` and `const` are block-scoped, meaning they are only accessible within the curly braces `{}`.
- Variables declared with `var` are attached to the `window` object, but `let` and `const` are not.

### Can you explain the difference between `==` and `===`?

- The `==` operator is called the equality operator, while the `===` operator is called the strict equality operator.
- Both operators check for equality, but `==` performs implicit type conversion, meaning it converts the operands to the same type before comparing them.
- In contrast, `===` does not perform any implicit type conversion and checks both the value and the type of the operands.

### Can you explain what higher-order functions are?

- Higher-order functions are functions that either accept another function as a parameter, return a function, or both.
- Example: The `forEach` method is a higher-order function because it takes another function as an argument.
### What is `Object.seal()` in JavaScript?

`Object.seal()` is a method in JavaScript that seals an object. It prevents the addition or deletion of properties while still allowing modifications to the values of existing properties. This is useful when you want to keep the object's structure the same but still allow updates to its data.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.seal(product);

product.company = "Apple"; // Not allowed, new properties cannot be added
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed, properties cannot be deleted
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max"; // Existing properties can be modified
console.log(product); // { name: "iPhone 14 Pro Max", price: 125000 }
```

### What is `Object.freeze()` in JavaScript?

`Object.freeze()` is a method in JavaScript that freezes an object, making it immutable. This means that no properties can be added, deleted, or modified. The object's structure and data remain exactly as they were when the object was frozen. This is useful when you want to prevent any changes to an object.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.freeze(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }
```

### What is `Object.preventExtensions()` in JavaScript?

`Object.preventExtensions()` is a method in JavaScript that prevents the addition of new properties to an object but allows the deletion and modification of existing properties' values.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.preventExtensions(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Allowed
console.log(product); // { name: "iPhone 14 Pro" }

product.name = "iPhone 14 Pro Max"; // Allowed
console.log(product); // { name: "iPhone 14 Pro Max" }
```

### What are `Object.isFrozen()` and `Object.isSealed()`?

These methods check if an object is frozen or sealed.

```javascript
console.log(Object.isFrozen(product)); // true or false
console.log(Object.isSealed(product)); // true or false
```

---

### What are the different types of scope in JavaScript?

The types of scope in JavaScript are:
   - **Global Scope**
   - **Function Scope**
   - **Block Scope**

#### 1. Global Scope

Variables declared outside any function or block are in the global scope. These variables are accessible from anywhere in your code, whether inside a function, loop, or conditional statement.

**Example:**

```javascript
let x = 10;

function showX() {
    console.log(x); // Accessible here
}

showX(); // Outputs: 10

console.log(x); // Accessible here too
```

In the above code, `x` is declared in the global scope, so it can be accessed both inside and outside the `showX` function.

#### 2. Function Scope

When a variable is declared inside a function, it is scoped to that function, meaning it can only be accessed within that function.

**Example:**

```javascript
function showY() {
    let y = 20;
    console.log(y); // Accessible here
}

showY(); // Outputs: 20

console.log(y); // Error: y is not defined
```

Here, `y` is declared inside the `showY` function, so it is only accessible within that function and not outside.

#### 3. Block Scope

Block scope is created when variables are declared inside a pair of curly braces `{}`. This can happen within loops, conditionals, or just standalone blocks.

**Example:**

```javascript
if (true) {
    let z = 30;
    console.log(z); // Accessible here
}

console.log(z); // Error: z is not defined
```

In this example, `z` is declared inside an `if` block, so it is only accessible within that block.

### What is the difference between `let`, `var`, and `const`?

- `let` and `const` were introduced in ES6, while `var` is from older versions of JavaScript.
- Variables declared with `var` are function-scoped, meaning they are accessible throughout the entire function. `var` can also be globally scoped. `var` does not support block scope.
- `let` and `const` are block-scoped, meaning they are only accessible within the curly braces `{}`.
- Variables declared with `var` are attached to the `window` object, but `let` and `const` are not.

### What is function scope? How does it differ from block scope?

**Function scope** refers to variables being scoped to a function, meaning they are accessible throughout the function. **Block scope** refers to variables being scoped to a block `{}`, meaning they are only accessible within that block.

### What is lexical (or static) scope? Can you explain it with an example?

JavaScript uses **lexical scoping**, also known as static scoping. In lexical scoping, scopes are allocated during compile time. So, in JavaScript, values for variables are assigned in the execution phase, but the scope of the variable is decided during the compilation phase.

**Example:**

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

### What happens if you try to access a variable that is not in the current scope?

In JavaScript, when you try to access a variable, JavaScript looks for it in the following order:

1. **Current Scope**: It first checks the current function or block scope.
2. **Outer Scopes**: If not found, it looks in the outer functions or blocks.
3. **Global Scope**: Finally, it checks the global scope.

If the variable isn't found in any of these scopes, you'll get a `ReferenceError`.

**Example:**

```javascript
let globalVar = 'I am global'; // Global scope

function outerFunction() {
    let outerVar = 'I am outer'; // Outer function scope

    function innerFunction() {
        let innerVar = 'I am inner'; // Inner function scope
        console.log(innerVar);  // Outputs: 'I am inner'
        console.log(outerVar);  // Outputs: 'I am outer'
        console.log(globalVar); // Outputs: 'I am global'
        console.log(nonExistentVar); // Throws ReferenceError
    }

    innerFunction();
}

outerFunction();
```

---

### Can you explain the difference between `==` and `===`?

- The `==` operator is called the **equality operator**, while the `===` operator is called the **strict equality operator**.
- Both operators check for equality, but `==` performs implicit type conversion, meaning it converts the operands to the same type before comparing them.
- In contrast, `===` does not perform any implicit type conversion and checks both the value and the type of the operands.

### Can you explain what higher-order functions are?

- **Higher-order functions** are functions that either accept another function as a parameter, return a function, or both.
- **Example:** The `forEach` method is a higher-order function because it takes another function as an argument.
