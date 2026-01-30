# Callback Hell & Pyramid of Doom

# Part 1: What are Callbacks?

- A callback is a function passed as an argument to another function,
  which is executed later (asynchronous tasks, event handling, etc.)

```js
// Example:
function fetchData(callback) {
  setTimeout(() => {
    callback("✅ Data fetched");
  }, 1000);
}
fetchData((result) => console.log(result));

//
// Problem: When multiple async tasks depend on each other → nested callbacks!
// This leads to "Callback Hell".
```

# Callback Hell

- Callback Hell = deeply nested callbacks, making code hard to read, debug, and maintain.
- Often called the "Pyramid of Doom" due to triangular nested structure.

```js
// Example:
setTimeout(() => {
  console.log("Step 1: User authenticated");
  setTimeout(() => {
    console.log("Step 2: User profile fetched");
    setTimeout(() => {
      console.log("Step 3: User orders fetched");
      setTimeout(() => {
        console.log("Step 4: Order details fetched");
      }, 1000);
    }, 1000);
  }, 1000);
}, 1000);

// Output (after delays):
// Step 1: User authenticated
// Step 2: User profile fetched
// Step 3: User orders fetched
// Step 4: Order details fetched
```

# ❌ Problems with Callback Hell:

- Hard to read (nested structure)
- Hard to debug
- Error handling is messy
- Code not reusable

# Callback Vs Promise

- Promise provide more structured way to handle async operations

- Callbacks lead to callback hell as well as (every callback needs it own error handling) complex to implement sequence or parallel flows

# Pyramid of Doom (Visualization)

```js
// Looks like this:

function task1() {
 task2(() => {
 task3(() => {
 task4(() => {
 task5(() => {
 ... endless nesting
    });
   });
 });
});
}

// Code structure visually resembles a "pyramid" 🔺

```

# Solving Callback Hell

```js
✅ 1) Use Named Functions
// Instead of nesting anonymous functions, create named ones.

function step1() {
console.log("Step 1");
setTimeout(step2, 1000);
}
function step2() {
console.log("Step 2");
setTimeout(step3, 1000);
}
function step3() {
console.log("Step 3");
}
setTimeout(step1, 1000);


 ✅ 2) Use Promises
// Promises flatten the nesting by using .then() chaining.

function fetchUser() {
return new Promise((resolve) =>
setTimeout(() => resolve("User Authenticated"), 1000),
);
}
function fetchProfile() {
return new Promise((resolve) =>
setTimeout(() => resolve("Profile Fetched"), 1000),
);
}
function fetchOrders() {
return new Promise((resolve) =>
setTimeout(() => resolve("Orders Fetched"), 1000),
);
}

// Chaining Promises
fetchUser()
.then((res) => {
console.log(res);
return fetchProfile();
})
.then((res) => {
console.log(res);
return fetchOrders();
})
.then((res) => {
console.log(res);
});

//
// ✅ 3) Use Async/Await
// Makes async code look synchronous.
//
async function getUserDetails() {
let user = await fetchUser();
console.log(user);

let profile = await fetchProfile();
console.log(profile);

let orders = await fetchOrders();
console.log(orders);
}
getUserDetails();

```

# Callback Hell by Akshay Saini

- There are 2 Parts of Callback:
  1. Good Part of callback - Callback are super important while writing asynchronous code in JS
  2. Bad Part of Callback - Using callback we can face issue:
     - Callback Hell
     - Inversion of control

- Understanding of Bad part of callback is super important to learn Promise in next lecture.

> 💡 JavaScript is synchronous, single threaded language. It can Just do one thing at a time, it has just one call-stack and it can execute one thing at a time. Whatever code we give to Javascript will be quickly executed by Javascript engine, it does not wait.

```js
console.log("Namaste");
console.log("JavaScript");
console.log("Season 2");
// Namaste
// JavaScript
// Season 2

// 💡 It is quickly printing because `Time, tide & Javascript waits for none.`
```

_But what if we have to delay execution of any line, we could utilize callback, How?_

```js
console.log("Namaste");
setTimeout(function () {
  console.log("JavaScript");
}, 5000);
console.log("Season 2");
// Namaste
// Season 2
// JavaScript

// 💡 Here we are delaying the execution using callback approach of setTimeout.
```

### 🛒 e-Commerce web app situation

Assume a scenario of e-Commerce web, where one user is placing order, he has added items like, shoes, pants and kurta in cart and now he is placing order. So in backend the situation could look something like this.

```js
const cart = ["shoes", "pants", "kurta"];
// Two steps to place a order
// 1. Create a Order
// 2. Proceed to Payment

// It could look something like this:
api.createOrder();
api.proceedToPayment();
```

Assumption, once order is created then only we can proceed to payment, so there is a dependency. So How to manage this dependency.
Callback can come as rescue, How?

```js
api.createOrder(cart, function () {
  api.proceedToPayment();
});
// 💡 Over here `createOrder` api is first creating a order then it is responsible to call `api.proceedToPayment()` as part of callback approach.
```

To make it a bit complicated, what if after payment is done, you have to show Order summary by calling `api.showOrderSummary()` and now it has dependency on `api.proceedToPayment()`
Now my code should look something like this:

```js
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary();
  });
});
```

Now what if we have to update the wallet, now this will have a dependency over `showOrderSummary`

```js
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet();
    });
  });
});
// 💡 Callback Hell
```

When we have a large codebase and multiple apis and have dependency on each other, then we fall into callback hell.
These codes are tough to maintain.
These callback hell structure is also known as **Pyramid of Doom**.

Till this point we are comfortable with concept of callback hell but now lets discuss about `Inversion of Control`. It is very important to understand in order to get comfortable around the concept of promise.

> 💡 Inversion of control is like that you lose the control of code when we are using callback.

Let's understand with the help of example code and comments:

```js
api.createOrder(cart, function () {
  api.proceedToPayment();
});

// 💡 So over here, we are creating a order and then we are blindly trusting `createOrder` to call `proceedToPayment`.

// 💡 It is risky, as `proceedToPayment` is important part of code and we are blindly trusting `createOrder` to call it and handle it.

// 💡 When we pass a function as a callback, basically we are dependant on our parent function that it is his responsibility to run that function. This is called `inversion of control` because we are dependant on that function. What if parent function stopped working, what if it was developed by another programmer or callback runs two times or never run at all.

// 💡 In next session, we will see how we can fix such problems.
```

> 💡 Async programming in JavaScript exists because callback exits.

# ❓ Interview Q&A

**Q1) What is Callback Hell?**
A situation when multiple async tasks are nested inside callbacks, creating unreadable "pyramid-shaped" code.

**Q2) Why is it bad?**
Hard to read, debug, maintain, and handle errors properly.

**Q3) How to avoid Callback Hell?**
Use named functions, Promises, Async/Await.

**Q4) What is Pyramid of Doom?**
The triangular shape of deeply nested callbacks.

**Q5) How do Promises/Async-Await solve callback hell?**
They flatten nested code into sequential, readable flow.

# ✅ Summary

- Callbacks are useful, but too many → Callback Hell.
- Callback Hell = Pyramid of Doom (deep nesting).
- Solutions: Named functions, Promises, Async/Await.
