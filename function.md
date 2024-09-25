## First-Class Functions

In JavaScript, functions are considered **first-class citizens**, meaning:

- We can **return** a function from another function.
- We can **pass** a function as an argument to another function.
- We can **store** a function in a variable.
- Functions are treated as **objects** and have associated methods, such as `call`, `apply`, and `bind`.

---

### Notes on Call Stack and Return Values

- Every entry created inside the call stack by a function call is called a **stack frame**.
- If a function does not explicitly return a value, JavaScript automatically returns `**undefined**`.

---

### Passing Values to Functions

- **Primitive Values**: When a **primitive value** (such as a number, string, or boolean) is passed to a function, the function receives a **copy** of that value. Changes made inside the function do **not** affect the original value.

- **Objects**: When an **object** (or array) is passed, the function receives a **reference** to the object. Changes to the object's properties inside the function **will** affect the original object. However, this is not true "pass by reference"—the reference itself is passed by value.

---

## Function Expressions

A function expression is created when the first valid token is not the `function` keyword.

```javascript
const myFun = function fun(x) {
    console.log("calling...", x);
};
myFun(10); // We can simply use the variable name and pass the required arguments in a pair of parentheses.
```

### Types of Function Expressions

#### Named Function Expression

A function expression with a name attached to it is called a named function expression.

```javascript
const myFun = function fun(x) {
    console.log("calling...", x);
};
```

Here, `fun` is the name of the function expression, and this function expression is stored in a variable `myFun`.

#### Anonymous Function Expression

A function expression without a name attached to it is called an anonymous function expression.

```javascript
const myFun = function (x) {
    console.log("calling...", x);
};
```

Here, the function expression has no name, and this function expression is stored in a variable called `myFun`.

#### Arrow Function Expression

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

#### Immediately Invoked Function Expression (IIFE)

- A function expression that is called the moment it is defined is called an IIFE.
- To define an IIFE, we first wrap a function inside a pair of parentheses and then immediately call the function by putting a subsequent pair of parentheses and passing arguments inside it.

```javascript
(function fun(x) {
    console.log("calling...", x);
})(10);

//Another example with arrow function
((x)=> console.log("calling...", x))(10);
```

- Here, the function `fun` is defined and immediately invoked with the argument `10`. The result is that "calling... 10" is logged to the console.
- Once we have executed the IIFE, it is wiped from memory because IIFE is used only once.
- IIFE can be very useful to avoid name conflicts because they don't interfere with variables in outer scopes. They can also be useful for temporary logic.

#### Function Expression as a Callback

Function expressions can also be passed as arguments to other functions, often used as callbacks.

```javascript
setTimeout(function (x) {
    console.log("calling after delay...", x);
}, 1000, 10);
```

Here, an anonymous function expression is passed as a callback to the `setTimeout` function, which will execute it after a delay of 1 second with the argument `10`.


---

### Named vs. Anonymous Function Expressions

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

## Higher-Order Functions and Callbacks

A `higher-order function` is a function that either takes another function as an argument, returns a new function, or both. The function used as an argument is called a `callback `function. By using higher-order functions, we can achieve a higher level of abstraction in our code, making it more modular and reusable.

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

---

### Data Transformation with `map()`, `filter()`, and `reduce()`

JavaScript provides powerful array methods for transforming and processing data, with three key methods being `map()`, `filter()`, and `reduce()`. These methods enable efficient and readable manipulation of arrays.
### Example Array:
```javascript
const numbers = [1, 2, 3, 4, 5];
```
### 1. **`map()`**: 
Applies a given function to each element of an array, returning a new array with the transformed values.
```javascript
const doubled = numbers.map(n => n * 2); // [2, 4, 6, 8, 10]
```
### 2. **`filter()`**: 
Creates a new array containing only the elements that satisfy the provided condition.
```javascript
const greaterThanTwo = numbers.filter(n => n > 2); // [3, 4, 5]
```
### 3. **`reduce()`**: 
Executes a reducer function (that you provide) on each element of the array, resulting in a single output value. Here, `acc` is the accumulator and `r` is the current value. `r` represents each value in the array, and `acc` represents the final result. The second argument of the `reduce` function is the initial value of `acc`.
```javascript
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15
```

---

### `call`, `apply`, and `bind` Methods

In JavaScript, `call`, `apply`, and `bind` are three very useful methods that allow you to control the value of the `this` keyword within a function.

#### `call`:
The `call` method allows you to explicitly set the value of `this` inside a function. It invokes the function with a specific `this` context and passes arguments individually.

```javascript
const myObj = {
    name: "Ram",
    greet: function(welcomeMessage, location) {
        console.log(`God ${this.name} ${welcomeMessage} to ${location}`);
    }
}
```

If you call `myObj.greet()`, `this` refers to `myObj`, so the name will be "Ram". To set `this` to another object, you can use the `call` method.

```javascript
const newObj = {
    name: "Krishna"
}

// Using call to set `this` to newObj and passing two arguments
myObj.greet.call(newObj, "Welcome", "Vrindavan"); 
// Output: God Krishna Welcome to Vrindavan
```

The `call` method allows us to pass the first argument as the new `this` context. If no object is passed, `this` refers to the global object. You can pass function parameters after the `this` reference.

#### `apply`:
The `apply` method is similar to `call`, but it takes two arguments:
1. The object to which `this` will refer.
2. An array of parameters to be passed to the function.

```javascript
// Using apply to set `this` to newObj and passing arguments as an array
myObj.greet.apply(newObj, ["Welcome", "Vrindavan"]);
// Output: God Krishna Welcome to Vrindavan
```

#### `bind`

The `bind` method is similar to `call`, but it does not invoke the function immediately. Instead, it creates a new function with `this` permanently bound to the specified value, allowing you to call this new function later.

```javascript
// Create a new function with `this` bound to newObj 
const boundGreet = myObj.greet.bind(newObj);

// Another bound function with one pre-set argument
const boundGreet2 = myObj.greet.bind(newObj, "came");

// Call the bound functions
boundGreet("Welcome", "Vrindavan"); // Output: God Krishna welcomes you to Vrindavan
boundGreet2("Dwarka Nagri"); // Output: God Krishna came to Dwarka Nagri
```

We also use `bind` in situations where we want to call a method of an object that relies on `this`, especially from an event listener. If we call the method directly, `this` will refer to the element to which the event listener is attached.

```javascript
const button = document.querySelector("button"); // Suppose a button in HTML
const myObj = {
    name: "God Krishna",
    greet: function(location) {
        console.log(`${this.name} welcomes you to ${location}`);
    }
};

// button.addEventListener("click", myObj.greet); 
// This will throw an error because `this` points to the button element.

// Solution using bind
const boundGreetButton = myObj.greet.bind(myObj); // Bind the greet method to myObj
button.addEventListener('click', function() {
    boundGreetButton('Vrindavan');
}); // Output: God Krishna welcomes you to Vrindavan
```
##### **Note**: 
Arrow functions do not work with `call`, `apply`, and `bind` as they do not have their own `this`. The `this` context of arrow functions is lexically scoped based on where the arrow function is defined.

--- 

### Closures
A `closure` provides access to the variables of its parent function even after that parent function has returned. The function keeps a reference to its outer scope, preserving the scope chain over time. This means that the variable environment of the execution context in which the function was created remains accessible even after that execution context has finished.

**Example**:
```JavaScript
const secureBooking = function() {
    let passengerCount = 0;
    return function() {
        passengerCount++;
        console.log(`${passengerCount} passengers`);
    }
}

const booker = secureBooking();

booker(); // 1 passengers
booker(); // 2 passengers
booker(); // 3 passengers
```
This example demonstrates how the inner function retains access to `passengerCount`, even after `secureBooking` has completed its execution.
