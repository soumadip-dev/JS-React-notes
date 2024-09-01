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
