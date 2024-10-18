## What are Promises?

Promises are a significant improvement over callbacks, offering a more structured and readable way to manage asynchronous operations. They are special JavaScript objects designed to handle tasks that are expected to complete in the future, such as downloading data or setting a timer.

**Focus Points:**
1. `How to create a promise.`
2. `How to consume a promise.`

---

## How to Create a Promise

- In JavaScript, a promise can be created using the `Promise` constructor. To do this, you call `Promise` with the `new` keyword and pass in a required parameter, resulting in a promise object.
- This constructor accepts a callback function as an argument, known as the `executor callback`.
- It is called the `executor callback` because it is executed immediately by the `Promise` constructor when the promise object is created. 
- Our responsibility is to define this `callback` function, which handles the asynchronous task.The `Promise` constructor takes responsibility for executing this callback.
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
- **OnRejected**[Hidden]

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

##### Example of a Promise

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

#### OnFulfillment and OnRejected

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
- If there are any tasks waiting in the runtime environment, they will be stored in the **macrotask queue**.
- Functions related to the promise's outcome (either success or failure) are stored in the **microtask queue**.
- The **microtask queue** has higher priority, so it runs before the **macro-task queue** once the call stack is empty.

### **Example Code**  
Here’s an example to illustrate how promises work in conjunction with other asynchronous code:

```javascript
// Create a promise that resolves after 500 ms
const promiseOne = new Promise(function executor1(resolve, reject) {
  console.log('Executor callback triggered by Promise constructor: promiseOne');
  
  setTimeout(function c1() {
    console.log('Timer of promiseOne done');
    resolve(100); // pending -> fulfilled; undefined -> 100
  }, 500);
});

// Handling the resolved or rejected value of promiseOne
promiseOne.then(
  function onSuccess() {
    console.log('onSuccess');
  },
  function onFailure() {
    console.log('onFailure');
  }
);

// Timer callback function for a separate 2-second timer
setTimeout(function timerCallback() {
  console.log('Timer 1 done');
}, 2000); // Timer of 2 seconds

// Create another promise that resolves or rejects based on a random number
const promiseTwo = new Promise(function executor2(resolve, reject) {
  console.log('Executor callback triggered by Promise constructor: promiseTwo');
  
  setTimeout(function promiseCallback() {
    const randomNumber = Math.floor(Math.random() * 100);
    console.log(randomNumber);
    
    if (randomNumber % 2 === 0) {
      resolve(randomNumber); // even number, resolve promise
    } else {
      reject(randomNumber); // odd number, reject promise
    }
  }, 3000);
});

// Handling the resolved or rejected value of promiseTwo
promiseTwo.then(
  function onSuccess(value) {
    console.log('Executing onSuccess for promiseTwo:', value);
  },
  function onFailure(value) {
    console.log('Executing onFailure for promiseTwo:', value);
  }
);

// Long-running synchronous task (not recommended in practice)
for (let i = 0; i < 1000000000; i++) {}
```

### **Output:**
```
Execution Start
Executor callback triggered by Promise constructor: promiseOne
Executor callback triggered by Promise constructor: promiseTwo
Timer of promiseOne done
onSuccess
Timer 1 done
86
Executing onSuccess for promiseTwo: 86
```

#### **Explanation:**

In this code, we can observe the following sequence of events:

- **promiseOne** is created by invoking the Promise constructor, which calls the `executor1` callback. It logs "Executor callback triggered by Promise constructor: promiseOne" (Output -> 1).
- A 500 ms timer is set for the `c1` callback. Since nothing else remains in `executor1`, it is removed from the call stack.
- The status of **promiseOne** is now "Pending," and its value is `undefined`.
- The `.then()` callbacks are registered for **promiseOne**:  
  - The `onSuccess` function is added to the fulfillment array of **promiseOne**.
  - The `onFailure` function is added to the rejection array of **promiseOne**.
- A separate 2-second timer is set to trigger `timerCallback` after 2000 ms.
- **promiseTwo** is created by calling a new Promise constructor, which is added to the call stack and invokes the `executor2` callback. It logs "Executor callback triggered by Promise constructor: promiseTwo" (Output -> 2).
- A 3-second timer is set for `promiseCallback`, which will either resolve or reject **promiseTwo** based on a random number. Since there is nothing more to do in the `executor2` callback, it is removed from the call stack.
- The status of **promiseTwo** is now "Pending," and its value is `undefined`.
- The `.then()` callbacks are registered for **promiseTwo**:  
  - The `onSuccess` function is added to the fulfillment array of **promiseTwo**.
  - The `onFailure` function is added to the rejection array of **promiseTwo**.
- A long loop runs for 1 billion iterations, blocking the event loop.
- In the background, the timer function for **promiseOne** completes first, adding `c1` to the macrotask queue. This is followed by the second timer adding `timerCallback`, and finally, the timer for **promiseTwo** completes, adding `promiseCallback` to the macrotask queue.
- After some time, the loop finishes, and the call stack becomes empty.
- The event loop checks if there is anything in the microtask queue. If none is found, it checks the macrotask queue. It finds the `c1` callback, moves it to the call stack, and executes it.
- `c1` first logs "Timer of promiseOne done" (Output -> 3) and resolves **promiseOne** with a value of 100. Now, the status of **promiseOne** is "fulfilled," and the `onSuccess` function from the fulfillment array is shifted to the microtask queue. After that, `c1` is removed from the call stack.
- The event loop now finds tasks in both the microtask and macrotask queues, but it prioritizes the microtask queue over the macrotask queue.
- It takes the `onSuccess` function from the microtask queue and executes it, logging "onSuccess" (Output -> 4).
- With nothing left in the microtask queue, the event loop checks the macrotask queue, finds `timerCallback`, and executes it, logging "Timer 1 done" (Output -> 5).
- It then finds `promiseCallback` in the macrotask queue and executes it. It first generates a random number and logs it:
  `randomnum` (Output -> 6).  
  Then it resolves or rejects the promise based on the random number.
  - If the random number is even, it resolves the promise and shifts the `onSuccess` function from the fulfillment array to the microtask queue, executing it and logging: "Executing onSuccess for promiseTwo: randomnum" (Output -> 7).
  - If the number is odd, it rejects the promise and logs: "Executing onFailure for promiseTwo: randomnum" (Output -> 7).

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

