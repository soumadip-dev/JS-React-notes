  ### Higher-Order Functions and Callbacks

A `higher-order function` is a function that either takes another function as an argument, returns a new function, or both. The function used as an argument is called a `callback` function. By using higher-order functions, we can achieve a higher level of abstraction in our code, making it more modular and reusable.

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

### Problem with Callbacks

While callbacks are powerful and allow us to write modular, reusable code, they can sometimes lead to issues when used extensively. There are two main problems with callbacks:

1. **Inversion of Control**: 
   When we pass a callback function to another function, we give up control over when and how that callback is executed. The function receiving the callback may call it immediately, later, multiple times, or not at all. This can sometimes lead to unexpected behavior and make debugging more challenging.

2. **Callback Hell** (also known as **Pyramid of Doom**): 
   When callbacks are nested within other callbacks, especially for asynchronous operations, it can lead to deeply nested code that is difficult to read and maintain. This "pyramid" shape can make code harder to follow and increase the risk of errors.
   
   Here's an example of callback hell:
   ```javascript
   doFirstTask(() => {
       doSecondTask(() => {
           doThirdTask(() => {
               doFourthTask(() => {
                   // Final task
               });
           });
       });
   });
   ```

---

### Synchronous and Asynchronous Programming

- In synchronous programming, tasks are executed one after another, and each task must complete before the next one begins. This is the default behavior of JavaScript.
- In asynchronous programming, tasks can be initiated without waiting for previous tasks to complete. This allows for more efficient execution, especially in tasks that involve waiting, like network requests or timers.

### What is the Default Nature of JavaScript?

JavaScript is **synchronous by default**. This means that any native JavaScript code executes line by line, with each line needing to complete before the next one can begin. This synchronous nature is due to JavaScript being single-threaded.

### Example of Asynchronous Code

```javascript
setTimeout(function f() {
  console.log("Hello");
}, 3000);

console.log("End");
```

In this example, `setTimeout` is a higher-order function that delays the execution of `console.log("Hello")` by 3000 milliseconds (3 seconds). However, the code after `setTimeout`, `console.log("End");`, executes immediately.

**Important Points**:

- `setTimeout` is not a native JavaScript function; it is provided by the runtime environment (e.g., Node.js, Deno, or web browsers like Chrome, Edge, etc.).
- Similarly, `console.log()` is also provided by the runtime environment, not by JavaScript itself.
- If you pass `0` as the delay time in `setTimeout`, `Node.js` automatically sets it to 1 millisecond. Setting a delay of `0` is not actually possible in `Node.js`.

---

### How Does JavaScript Handle Runtime Code?

JavaScript manages runtime code using the **event loop** and **queues** (like the macro task queue and micro task queue). Here's a simplified explanation:

1. **Call Stack**: JavaScript has a call stack where it keeps track of the function calls. If a task in the code is related to the runtime environment (like `setTimeout`), JavaScript sends it to the runtime environment and moves on to the next line of code.
2. **Event Loop**: The event loop continuously checks if the call stack is empty. If the call stack is empty and there are tasks waiting in the queue (like a completed `setTimeout`), the event loop moves the task from the queue to the call stack for execution.
3. **Queues**: If a runtime task completes while the call stack is not empty, the runtime environment places the completed tasks into the `Macro-task queue`. The tasks in these queues are processed in a **First-Come, First-Served (FCFS)** order when the call stack is empty.

In summary, the event loop ensures that tasks are executed efficiently, even when asynchronous operations are involved.

---

### Closures

A `closure` provides access to the variables of its parent function even after that parent function has returned. The function keeps a reference to its outer scope, preserving the scope chain over time. This means that the variable environment of the execution context in which the function was created remains accessible even after that execution context has finished.

**Example**:
```javascript
const secureBooking = function() {
    let passengerCount = 0;
    return function() {
        passengerCount++;
        console.log(`${passengerCount} passengers`);
    };
};

const booker = secureBooking();

booker(); // 1 passengers
booker(); // 2 passengers
booker(); // 3 passengers
```

This example demonstrates how the inner function retains access to `passengerCount`, even after `secureBooking` has completed its execution.

---

## What are Promises?

Promises are a significant improvement over callbacks, offering a more structured and readable way to manage asynchronous operations. They are special JavaScript objects designed to handle tasks that are expected to complete in the future, such as downloading data or setting a timer.

**Focus Points**:
1. `How to create a promise.`
2. `How to consume a promise.`

---

## How to Create a Promise

- In JavaScript, a promise can be created using the `Promise` constructor. To do this, you call `Promise` with the `new` keyword and pass in a required parameter, resulting in a promise object.
- This constructor accepts a callback function as an argument, known as the `executor callback`.
- It is called the `executor callback` because it is executed immediately by the `Promise` constructor when the promise object is created.
- Our responsibility is to define this `callback` function, which handles the asynchronous task. The `Promise` constructor takes responsibility for executing this callback.
- The callback function accepts two parameters: `resolve` and `reject`, which are functions provided by the `Promise` constructor.

```javascript
new Promise((resolve, reject) => {
  // resolve -> resolver, reject -> rejecter
});
```

- Since the `Promise` constructor is responsible for executing the callback, it also provides the `resolve` and `reject` functions that you can define within the callback to handle the success or failure of the asynchronous task.

---

### What Does a Promise Object Look Like?

A promise object contains:

- **Status**: The current state of the promise.
- **Value**: The result of the promise.
- **OnfulFillment** [Hidden]
- **OnRejected** [Hidden]

#### Promise Status

A promise object can be in one of three states:

1. **Pending**: The initial state when the promise is created.
2. **Fulfilled**: The state when the operation inside the promise has completed successfully.
3. **Rejected**: The state when the operation inside the promise has failed.

- When a promise is created, it starts in the **pending** state.
- It transitions to either the **fulfilled** state (if `resolve` is called) or the **rejected** state (if `reject` is called).
- The transition from pending to fulfilled or rejected happens only once. Once a promise changes state, it cannot change again.

#### Promise Value

The promise object also contains a value, referred to as the "promise result." This value holds the outcome of the operation performed within the promise:

- If the promise is **fulfilled**, the value is the result of the successful operation.
- If the promise is **rejected**, the value is the error or reason for the failure.

#### Example of a Promise

```javascript
const pr = new Promise((resolve, reject) => {
  setTimeout(() => {
    const randomNum = Math.floor(Math.random() * 100);
    console.log(randomNum);

    if (randomNum % 2 === 0) {
      resolve(randomNum);
    } else {
      reject(randomNum);
    }
  }, 5000);
});

console.log(pr);
```

**Output:**
```
Promise {<pending>}

// After 5 seconds
13 (random)
Promise {<rejected>: 13}
```

### OnFulfillment and OnRejected

- These are internal arrays managed by the promise object, initially empty.
- Functions can be stored in these arrays using the `.then()` or `.catch()` methods on the promise object.
- If the promise is in the **pending** state, the functions remain in their respective arrays.
- When the promise transitions to the **fulfilled** state, all functions in the `OnFulfillment` array are queued in the **Microtask queue**, while the `OnRejected` array does nothing.
- When the promise transitions to the **rejected** state, all functions in the `OnRejected` array are queued in the **Microtask queue**, while the `OnFulfillment` array does nothing.

---

## How to Consume a Promise

- Promises act as a placeholder object for something that will be available in the future.
- Once the future execution is done, we might want to execute some code based on the outcome.
- **Using `.then()` to Handle Promises**  
  To execute code after a promise is resolved or rejected, we can use the `.then()` method. This method takes two arguments:
  1. `onFulfillment`: A function to handle the success of the promise (required).
  2. `onRejected`: A function to handle the failure of the promise (optional).

```javascript
const pr = new Promise((resolve, reject) => {
  // Your code here
});

pr.then(onFulfillmentFunction, onRejectedFunction); 
// --> onFulfillmentFunction goes to the OnFulfillment array 
// --> onRejectedFunction goes to the OnRejected array
```

- **Promise Resolution Process**  
  - If the promise is fulfilled (successful), the functions passed as `onFulfillment` are executed.
  - If the promise is rejected (failed), the functions passed as `onRejected` are executed.
  - If there's any task waiting in the runtime environment, it will be stored in the `macrotask queue`.
  - Functions related to the promise's outcome (either success or failure) are stored in the `microtask queue`.
  - The `microtask queue` has higher priority, so it runs before the `macrotask queue` once the call stack is empty.

- **Example Code**  
  Here's an example to illustrate how promises work in conjunction with other asynchronous code:

  ```javascript
  console.log("Execution Start");

  setTimeout(() => {
    console.log("First Timer Completed");
  }, 5000);

  const generateRandomNumberPromise = new Promise((resolve, reject) => {
    console.log("Promise Execution Started");

    setTimeout(() => {
      const randomNumber = Math.floor(Math.random() * 100);
      console.log(`Generated Number: ${randomNumber}`);

      if (randomNumber % 2 === 0) {
        resolve("Even number");
      } else {
        reject("Odd number");
      }
    }, 100);
  });

  generateRandomNumberPromise.then(
    () => console.log("Even number handler executed"),
    () => console.log("Odd number handler executed")
  );

  generateRandomNumberPromise.then(
    () => console.log("Success handler executed"),
    () => console.log("Failure handler executed")
  );

  // Simulate a long-running process
  for (let i = 0; i < 1000000000; i++) {
    // Simulating an intensive task
  }

  console.log("Execution End");
  ```

In this code, you can observe the following:
- The `Promise Execution Started` message is logged immediately after `Execution Start`.
- The `Generated Number` and either `Even number` or `Odd number` are logged after a short delay.
- The promise's `.then()` handlers execute based on the resolved or rejected state of the promise.
- Despite the long-running `for` loop, the promise's resolution handlers (`.then()` callbacks) execute before the loop completes, demonstrating the priority of the microtask queue over the `macrotask` queue.

---

### More on the `.then()` Function

- **Behaviour of `.then()` Function:**  
  The `.then()` function returns a new promise, which is different from the original promise on which it was called.

  ```javascript
  const p1 = new Promise((res, rej) => {
    setTimeout(() => {
      res("p1 resolved");
    }, 2000);
  });

  const p2 = p1.then(
    function f() {
      return "Function F";
    },
    function g() {
      console.log("Function G");
    }
  );
  ```

  **Explanation:**  
  - When `p1.then()` returns a new promise `p2`, initially, `p2` behaves like any other promise, with a value of `undefined` and a status of `pending`.
  - The resolution or rejection of `p2` is handled by the functions `f()` or `g()` provided inside the `.then()` function.
  - When `f()` or `g()` returns something, the status of `p2` will be `fulfilled`, and the return value is set to the value of `p2`.
  - If `f()` throws an exception, the status of `p2` will be `rejected`.
  - If another promise is returned from the function `f()`, then the resolution or rejection of `p2` will be dependent on the outcome of that returned promise. 
    - If the returned promise is fulfilled, `p2` will also be fulfilled with the same value.
    - If the returned promise is rejected, `p2` will be rejected with the same reason.

- **Chaining Promises:**  
  `.then()` can be chained to handle a sequence of asynchronous operations, where the result of one operation is passed to the next. Each `.then()` in the chain returns a new promise, allowing for sequential asynchronous processing.

  ```javascript
  function downloadData(url) {
    return new Promise(function executeDownload(resolve, reject) {
      console.log(`Downloading from ${url}`);

      setTimeout(() => {
        console.log("Download is complete");
        let downloadedData = "Some Data";
        resolve(downloadedData);
      }, 3000);
    });
  }

  function writeDataToFile(data, fileName) {
    return new Promise(function executeWrite(resolve, reject) {
      console.log(`Writing ${data} to file`);

      setTimeout(() => {
        console.log(`Writing to file ${fileName} is complete`);
        resolve(null);
      }, 2000);
    });
  }

  function uploadFile(fileName, url) {
    return new Promise(function executeUpload(resolve, reject) {
      console.log(`Uploading ${fileName} to ${url}`);

      setTimeout(() => {
        console.log("Upload is complete");
        let uploadStatus = "Success";
        resolve(uploadStatus);
      }, 1000);
    });
  }

  downloadData("https://example.com/data")
    .then(function handleDownload(downloadedData) {
      return writeDataToFile(downloadedData, "file.txt");
    })
    .then(function handleWrite() {
      return uploadFile("file.txt", "https://example.com/upload");
    })
    .then(function handleUpload(status) {
      console.log(status);
    });
  ```

  **Explanation:**  
  In the above example, each `.then()` receives the result of the previous one and performs an operation on it, passing the new value to the next `.then()`.

---
