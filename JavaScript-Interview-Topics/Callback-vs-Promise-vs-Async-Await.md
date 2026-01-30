# Compare: Callbacks vs Promises vs Async/Await

# Why compare?

- These are 3 different ways to handle asynchronous code in JavaScript.

# 1) CALLBACKS

- A function passed as an argument to another function.
- Called when the task is complete.
- Problem: "Callback Hell" (deep nesting, hard to read).

```js
// Example:
function getDataCallback(callback) {
  setTimeout(() => {
    callback("✅ Data received (Callback)");
  }, 1000);
}

getDataCallback((data) => {
  console.log(data);
});

// Output (after 1s): ✅ Data received (Callback)
```

# 2) PROMISES

- Cleaner alternative to callbacks.
- A Promise represents a value that may be available now, later, or never.
- Uses `.then()` for success and `.catch()` for errors.

```js
// Example:
function getDataPromise() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("✅ Data received (Promise)");
    }, 1000);
  });
}

getDataPromise()
  .then((data) => console.log(data))
  .catch((err) => console.log(err));

// Output (after 1s): ✅ Data received (Promise)
```

# 3) ASYNC / AWAIT

- Built on top of Promises (syntactic sugar).
- Makes async code look like synchronous code.
- Use `try/catch` for error handling.

```js
// Example:
async function getDataAsync() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("✅ Data received (Async/Await)");
    }, 1000);
  });
}

async function showData() {
  try {
    let data = await getDataAsync();
    console.log(data);
  } catch (err) {
    console.log("❌ Error:", err);
  }
}

showData();

// Output (after 1s): ✅ Data received (Async/Await)
```

# Quick Comparison Table

| Feature         | Callbacks               | Promises               | Async / Await         |
| --------------- | ----------------------- | ---------------------- | --------------------- |
| Syntax          | Nested functions        | `.then()` / `.catch()` | Looks synchronous     |
| Error Handling  | Inside callback         | `.catch()`             | `try / catch`         |
| Readability     | ❌ Hard (Callback Hell) | 🙂 Better              | ✅ Best               |
| Execution Order | Manual nesting          | Chained                | Sequential / Parallel |
| Based On        | Functions               | Objects                | Promises              |

---

## ❓ Interview Q&A

**Q1) Why did Promises replace Callbacks?**  
👉 To solve **Callback Hell** and provide better error handling.

**Q2) Is async/await same as Promises?**  
👉 Yes, **async/await is syntactic sugar over Promises**.

**Q3) Which one is best for readability?**  
👉 **Async / Await**.

**Q4) How do you handle errors in different approaches?**

- **Callbacks** → Pass error as **1st argument**
- **Promises** → Use `.catch()`
- **Async / Await** → Use `try / catch`

**Q5) Can you mix Promises and Async/Await?**  
👉 Yes. Example: `await Promise.all([...])`

---

## ✅ Easy to Remember

- **Callbacks** → Old, messy, _Callback Hell_
- **Promises** → Cleaner with `.then()` / `.catch()`
- **Async / Await** → Modern, looks synchronous, **best readability**

---
