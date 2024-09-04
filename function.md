## First-Class Functions

In JavaScript, functions are first-class citizens, meaning:
- We can return a function from a function.
- We can pass a function as an argument to another function.
- We can also store a function in a variable.

---

**Note:**
- Every entry created inside the call stack by a function call is called a stack frame.
- If we do not return anything from a function, JavaScript will automatically return `undefined`.

---

## Function Expressions

When we create a function, but the first valid token is not the `function` keyword, we call this type of instruction a function expression.

```javascript
const myFun = function fun(x) {
    console.log("calling...", x);
};
myFun(10); // We can just give the variable name and pass the required args in a pair of parentheses
```

### Different Ways to Define Function Expressions

#### Example 1: Named Function Expression

A function expression with a name attached to it is called a named function expression.

```javascript
const myFun = function fun(x) {
    console.log("calling...", x);
};
```

Here, `fun` is the name of the function expression, and this function expression is stored in a variable `myFun`.

#### Example 2: Anonymous Function Expression

A function expression without a name attached to it is called an anonymous function expression.

```javascript
const myFun = function (x) {
    console.log("calling...", x);
};
```

Here, the function expression has no name, and this function expression is stored in a variable called `myFun`.

#### Example 3: Arrow Function Expression

- An arrow function expression is a more concise way to write function expressions using the `=>` syntax.
- One fundamental difference between arrow functions and other function expressions is that in arrow functions, the `this` keyword is resolved lexically, whereas in other function expressions, the `this` keyword is resolved by the call site.

```javascript
const cube = (x) => {
    return x ** 3;
};
```

- In the code below, the only thing `square` is expected to do is return `x * x`; for an arrow function, we don't need to write the `return` keyword.

```javascript
const square = (x) => x * x; // Implicit return
```

- If the arrow function takes only a single argument, then parentheses are not mandatory.

```javascript
const square = x => x * x;
```

- If there is more than one or no argument, then parentheses are required.
- If we try to return an object, we need to put the object inside parentheses.

```javascript
const fun = () => ({ username: "Soumadip" });
```

- Example of using an arrow function in `map` for an array:

```javascript
arr.map(x => x * x);
```

#### Example 4: Immediately Invoked Function Expression (IIFE)

- A function expression that is called the moment it is defined is called an IIFE.
- To define an IIFE, we first wrap a function inside a pair of parentheses and then immediately call the function by putting a subsequent pair of parentheses and passing arguments inside it.

```javascript
(function fun(x) {
    console.log("calling...", x);
})(10);
```

- Here, the function `fun` is defined and immediately invoked with the argument `10`. The result is that "calling... 10" is logged to the console.
- Once we have executed the IIFE, it is wiped from memory because IIFE is used only once.
- IIFE can be very useful to avoid name conflicts because they don't interfere with variables in outer scopes. They can also be useful for temporary logic.

#### Example 5: Function Expression as a Callback

Function expressions can also be passed as arguments to other functions, often used as callbacks.

```javascript
setTimeout(function (x) {
    console.log("calling after delay...", x);
}, 1000, 10);
```

Here, an anonymous function expression is passed as a callback to the `setTimeout` function, which will execute it after a delay of 1 second with the argument `10`.

### Named vs. Anonymous Function Expression

- **Readability:** Named function expressions improve the readability of the code. Anonymous function expressions require reading the whole logic to understand what they do.
- **Debugging:** Anonymous functions are hard to debug because, in the call stack, they do not have a name.

```javascript
function fun(fn) {
    const arr = [1, 2, 3, 4, 5];
    fn(arr);
}

fun(function gun(arr) {
    console.trace("call stack"); // This function prints the call stack in the console
});
```

Output:
```
call stack
gun @ VM232:7
fun @ VM232:3
(anonymous) @ VM232:6
```

- **Recursion:** Named function expressions can be easily integrated into a recursive environment, whereas anonymous functions are hard to use in recursion.
- **Anonymous function expressions** are useful when:
  - The code logic is simple.
  - We are storing the anonymous function expression in a variable, and that variable explains most of the logic of the function expression.

---

## Higher-Order Functions

A function that takes another function as an argument or returns a function is known as a higher-order function.

```javascript
const radius = [1, 2];
const area = function (radius) {
    return Math.PI * radius * radius;
};
const circumference = function (radius) {
    return 2 * Math.PI * radius;
};
const calculate = function (radius, logic) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(logic(radius[i]));
    }
    return output;
};

console.log(calculate(radius, area)); // [3.141592653589793, 12.566370614359172]
console.log(calculate(radius, circumference)); // [6.283185307179586, 12.566370614359172]
```

- `map` is also a higher-order function in arrays. Instead of `calculate`, we can use the `map` function:

```javascript
console.log(radius.map(area)); // [3.141592653589793, 12.566370614359172]
```

### Notes on `map`, `filter`, and `reduce`:

- **`map`:** Applies a given function to each element of an array, returning a new array with the results.

```javascript
const doubled = radius.map(r => r * 2); // [2, 4]
```

- **`filter`:** Creates a new array with all elements that pass a test implemented by the provided function.

```javascript
const greaterThanOne = radius.filter(r => r > 1); // [2]
```

- **`reduce`:** Executes a reducer function (that you provide) on each element of the array, resulting in a single output value. Here, `acc` is the accumulator and `r` is the current value. `r` represents each value in the array, and `acc` represents the final result. The second argument of the `reduce` function is the initial value of `acc`.

```javascript
const sum = radius.reduce((acc, r) => acc + r, 0); // 3
```

---
