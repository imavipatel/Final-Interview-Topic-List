# Promise Chaining

- We can return a new Promise from .then()
- Each .then() returns a new promise automatically.

```js
// Example: Sequential execution
function step1() {
  return new Promise((resolve) =>
    setTimeout(() => resolve("Step 1 complete"), 1000),
  );
}
function step2() {
  return new Promise((resolve) =>
    setTimeout(() => resolve("Step 2 complete"), 1000),
  );
}
function step3() {
  return new Promise((resolve) =>
    setTimeout(() => resolve("Step 3 complete"), 1000),
  );
}

step1()
  .then((res) => {
    console.log(res);
    return step2();
  })
  .then((res) => {
    console.log(res);
    return step3();
  })
  .then((res) => {
    console.log(res);
  })
  .catch((err) => {
    console.log("Error occurred:", err);
  });

// Output:
// Step 1 complete
// Step 2 complete
// Step 3 complete
//
// -----------------------------------------------------------------------------
// Part 5: Returning Values in Chain
// -----------------------------------------------------------------------------
new Promise((resolve) => resolve(10))
  .then((num) => {
    console.log("First:", num); // 10
    return num * 2;
  })
  .then((num) => {
    console.log("Second:", num); // 20
    return num * 2;
  })
  .then((num) => {
    console.log("Third:", num); // 40
  });

// -----------------------------------------------------------------------------
// Part 6: Error Handling in Chaining
// -----------------------------------------------------------------------------
new Promise((_, reject) => reject("❌ Something went wrong"))
  .then((res) => {
    console.log("This will not run:", res);
  })
  .catch((err) => {
    console.log("Caught error:", err);
    return "Recovered value";
  })
  .then((res) => {
    console.log("After recovery:", res);
  });

// Output:
// Caught error: ❌ Something went wrong
// After recovery: Recovered value
```

# Parallel Execution (vs Chaining)

- Chaining = sequential execution.
- Promise.all = parallel execution.

```js
function asyncTask(ms) {
  return new Promise((resolve) =>
    setTimeout(() => resolve(`Task ${ms} done`), ms),
  );
}

Promise.all([asyncTask(1000), asyncTask(2000), asyncTask(1500)]).then(
  (results) => console.log("Parallel Results:", results),
);

// Output after ~2s:
// Parallel Results: [ 'Task 1000 done', 'Task 2000 done', 'Task 1500 done' ]
```

# Promise chaining by Akshay Saini

```js
const cart = ["shoes", "pants", "kurta"];

// Consumer part of promise
const promise = createOrder(cart); // orderId
// Our expectation is above function is going to return me a promise.

promise.then(function (orderId) {
  proceedToPayment(orderId);
});

// Above snippet we have observed in our previous lecture itself.
// Now we will see, how createOrder is implemented so that it is returning a promise
// In short we will see, "How we can create Promise" and then return it.

// Producer part of Promise
function createOrder(cart) {
  // JS provides a Promise constructor through which we can create promise
  // It accepts a callback function with two parameter `resolve` & `reject`
  const promise = new Promise(function (resolve, reject) {
    // What is this `resolve` and `reject`?
    // These are function which are passed by javascript to us in order to handle success and failure of function call.
    // Now we will write logic to `createOrder`
    /** Mock logic steps
     * 1. validateCart
     * 2. Insert in DB and get an orderId
     */
    // We are assuming in real world scenario, validateCart would be defined
    if (!validateCart(cart)) {
      // If cart not valid, reject the promise
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345"; // We got this id by calling to db (Assumption)
    if (orderId) {
      // Success scenario
      resolve(orderId);
    }
  });
  return promise;
}
```

Over above, if your validateCart is returning true, so the above promise will be resolved (success),

```js
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart); // orderId
// ❓ What will be printed in below line?
// It prints Promise {<pending>}, but why?
// Because above createOrder is going to take sometime to get resolved, so pending state. But once the promise is resolved, `.then` would be executed for callback.
console.log(promise);

promise.then(function (orderId) {
  proceedToPayment(orderId);
});

function createOrder(cart) {
  const promise = new Promise(function (resolve, reject) {
    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345";
    if (orderId) {
      resolve(orderId);
    }
  });
  return promise;
}
```

Now let's see if there was some error and we are rejecting the promise, how we could catch that?  
-> Using `.catch`

```js
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart); // orderId

// Here we are consuming Promise and will try to catch promise error
promise
  .then(function (orderId) {
    // ✅ success aka resolved promise handling
    proceedToPayment(orderId);
  })
  .catch(function (err) {
    // ⚠️ failure aka reject handling
    console.log(err);
  });

// Here we are creating Promise
function createOrder(cart) {
  const promise = new Promise(function (resolve, reject) {
    // Assume below `validateCart` return false then the promise will be rejected
    // And then our browser is going to throw the error.
    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345";
    if (orderId) {
      resolve(orderId);
    }
  });
  return promise;
}
```

Now, Let's understand the concept of Promise Chaining  
-> for this we will assume after `createOrder` we have to invoke `proceedToPayment`  
-> In promise chaining, whatever is returned from first `.then` become data for next `.then` and so on...  
-> At any point of promise chaining, if promise is rejected, the execution will fallback to `.catch` and others promise won't run.

```js
const cart = ["shoes", "pants", "kurta"];

createOrder(cart)
  .then(function (orderId) {
    // ✅ success aka resolved promise handling
    // 💡 we have return data or promise so that we can keep chaining the promises, here we are returning data
    console.log(orderId);
    return orderId;
  })
  .then(function (orderId) {
    // Promise chaining
    // 💡 we will make sure that `proceedToPayment` returns a promise too
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    // from above, `proceedToPayment` is returning a promise so we can consume using `.then`
    console.log(paymentInfo);
  })
  .catch(function (err) {
    // ⚠️ failure aka reject handling
    console.log(err);
  });

// Here we are creating Promise
function createOrder(cart) {
  const promise = new Promise(function (resolve, reject) {
    // Assume below `validateCart` return false then the promise will be rejected
    // And then our browser is going to throw the error.
    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345";
    if (orderId) {
      resolve(orderId);
    }
  });
  return promise;
}

function proceedToPayment(cart) {
  return new Promise(function (resolve, reject) {
    // For time being, we are simply `resolving` promise
    resolve("Payment Successful");
  });
}
```

Q: What if we want to continue execution even if any of my promise is failing, how to achieve this?  
-> By placing the `.catch` block at some level after which we are not concerned with failure.  
-> There could be multiple `.catch` too.
Eg:

```js
createOrder(cart)
  .then(function (orderId) {
    // ✅ success aka resolved promise handling
    // 💡 we have return data or promise so that we can keep chaining the promises, here we are returning data
    console.log(orderId);
    return orderId;
  })
    .catch(function (err) {
    // ⚠️ Whatever fails below it, catch wont care
    // this block is responsible for code block above it.
    console.log(err);
  });
  .then(function (orderId) {
    // Promise chaining
    // 💡 we will make sure that `proceedToPayment` returns a promise too
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    // from above, `proceedToPayment` is returning a promise so we can consume using `.then`
    console.log(paymentInfo);
  })
```

# Interview Q&A

**Q1) What is a Promise?**
👉 A wrapper for asynchronous operations that can be pending, resolved, or rejected.

**Q2) Why are Promises better than callbacks?**
👉 Cleaner syntax, better error handling, avoid Callback Hell, composable.

**Q3) What is Promise Chaining?**
👉 Linking multiple .then() calls, where each step runs after the previous resolves.

**Q4) How to handle errors in chaining?**
👉 Using .catch() at the end (or after any .then()).

**Q5) Difference between sequential & parallel execution in Promises?**
👉 Sequential → chaining (one after another).
/👉 Parallel → Promise.all (all run together).

# ✅ Summary

- Promises handle async operations better than callbacks.
- Promise chaining = sequential execution of async tasks.
- Use .then(), .catch(), .finally().
- Parallel execution with Promise.all.
- Async/Await makes it even more readable.
