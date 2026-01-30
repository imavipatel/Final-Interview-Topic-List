**What are Web Workers**

- **Web Workers** are a feature of the **Web API** that allows JavaScript code to run in the **background** on a **separate thread.** This enables multithreading-like behavior, improving performance by offloading resource-intensive tasks from the main thread.

- This means you can perform expensive operations like large data processing, image transformations, or complex calculations without freezing the UI.

- Web Workers operate in a **different execution context**, meaning they do not have access to the **DOM, window, or document objects.** However, they can communicate with the main thread via **messages (postMessage).**

- Helps improve **performance** and **responsiveness**.

## Web Workers Why it is needed?

JavaScript runs code in a single sequence, which is called single-threaded. This design works well for simple tasks in web browsers, but it can cause problems when the main thread is blocked by heavy tasks, like complex calculations or background operations. These tasks can make the page slow and unresponsive. To solve this, JavaScript offers Web Workers, which allow you to move heavy tasks to a separate thread, keeping the main thread free for smooth user interactions.

**Creating a Web Worker**

- Create a worker using `new Worker("worker.js")`
- The worker runs code in `worker.js` independently.

**Example: worker.js**
Create a JavaScript file, such as worker.js, containing the code to be executed.

```js
self.addEventListener("message", (event) => {
  const data = event.data;
  // Do some heavy computation
  let sum = 0;
  for (let i = 0; i < data; i++) sum += i;
  self.postMessage(sum); // send result back
});
```

**Main.js**
In the main page’s JavaScript, instantiate the Web Worker and initiate communication.

```js
const worker = new Worker("worker.js");
worker.postMessage(1000000); // send data to worker
worker.addEventListener("message", (event) => {
  console.log("Result from worker:", event.data); // sum of 0..999999
});

// Error handling
worker.addEventListener("error", (error) => {
  console.error("Worker error:", error);
});
```

**Communicating with Web Workers**

- `postMessage(data)` → send data to worker

- Worker uses `postMessage(result)` → send data back

- `onmessage` or `addEventListener("message")` → listen for messages

```js
worker.onmessage = (event) => {
  console.log("Worker replied:", event.data);
};
```

**Terminating Web Workers**

- To avoid memory leaks, **always terminate workers when done**.
- `worker.terminate()` → stops the worker

**Example:**

```js
worker.terminate();
```

**Avoiding Memory Leaks**

1. Always terminate workers when no longer needed
2. Remove message listeners if worker will persist
3. Avoid passing large objects repeatedly; serialize/transfer if possible
4. Keep background computation light and finite

```js
// Example: Proper cleanup
const worker2 = new Worker("worker.js");
worker2.postMessage(500000);
const handleMsg = (event) => console.log("Worker2 result:", event.data);
worker2.addEventListener("message", handleMsg);

// After completion
setTimeout(() => {
  worker2.removeEventListener("message", handleMsg);
  worker2.terminate();
  console.log("Worker2 terminated and cleaned up.");
}, 2000);
```

**🔑 Key Points Summary**

1. Web Workers run JS in a **separate thread**, preventing UI blocking
2. Cannot access DOM directly; use messages for communication
3. Always terminate workers to **avoid memory leaks**
4. Good for **heavy computations, long loops, or async tasks**
5. Use message listeners carefully to prevent lingering references

## Q & A

**Q1) Can a Web Worker access the DOM directly?**

No, workers run in a separate thread without access to DOM.

**Q2) How do we communicate with a worker?**
Using `postMessage(data)` and listening with `onmessage`.

**Q3) What happens if we don’t terminate a worker?**
Worker continues running in background → can cause memory leaks.

**Q4) Can we have multiple workers?**
Yes, multiple workers can run in parallel for separate tasks.

**Q5) When should we use Web Workers?**

For CPU-intensive tasks, like image processing, large computations, or async data processing, to keep UI responsive.

## Benifits of Web Workers

**Preventing UI Freezing:** The main thread continues to handle rendering, animations, and user interactions.
**Improving Perceived Performance:** Users see a responsive application, even if heavy computations are happening in the background.
**Enhancing User Experience:** No more "unresponsive script" warnings.

## Web Workers vs. Asynchronous Operations (e.g., async/await, fetch, setTimeout)

This is a crucial distinction. While async/await and Web APIs like fetch make your code asynchronous, they do not run JavaScript in parallel threads (for computation).

**Asynchronous Web APIs (fetch, setTimeout, XMLHttpRequest):**

**Purpose:** Primarily for I/O-bound tasks (Input/Output), like waiting for network responses or a timer to expire.

**Execution:** The main thread initiates the I/O operation. The browser's underlying multi-threaded C++ infrastructure handles the waiting. Once the I/O is complete, a callback is placed on the Event Queue to be processed by the main thread when it's available.

**Blocking:** The main thread is non-blocking while waiting for I/O. However, if the callback function itself contains heavy computation, that computation will block the main thread when it finally runs.

## async/await:

**Purpose:** A syntax sugar to make asynchronous I/O-bound code appear synchronous and easier to manage.

**Execution:** Still operates on the single main thread. await pauses the function execution, allowing the main thread to do other work while waiting for a Promise to resolve (typically an I/O operation).

**Blocking:** If you have a long, CPU-intensive loop or calculation inside an async function (not behind an await for an I/O operation), that specific calculation **will block the main thread** until it's finished. async/await helps with scheduling, not parallel computation.

## Web Workers:

**Purpose:** Specifically for CPU-bound tasks (heavy computations, complex algorithms).
**Execution:** The entire JavaScript function for the heavy computation runs in a completely separate, isolated thread.
**Blocking:** The main thread is never blocked by the worker's computation. It truly runs in parallel.

## The Golden Rule:

Use async/await for **waiting** (I/O operations).
Use Web Workers for **calculating** (CPU-intensive operations).

## Types of Web Workers

There are a few specialized types of Web Workers:

**Dedicated Workers:**

- The most common type.
- Created by a single script and owned by that script.
- Terminated when the parent script terminates or explicitly by `worker.terminate()`.
- Each instance is tied to one specific owner.

**Shared Workers:**

- Can be accessed by multiple scripts, even across different browser tabs or iframes, as long as they are from the same origin.
- More complex to set up due to port management for communication.
- They remain active as long as at least one script is connected to them.

**Service Workers:**

- A specialized type of Web Worker with a unique lifecycle and capabilities.
- **Primary Purpose:** Acts as a **network proxy** between the browser and the network.
- **Key Features:** Enables offline experiences, custom caching strategies, push notifications, and background synchronization.
- **Lifecycle:** Independent of the web page that registered it; can run even when the page is closed.
- **Scope:** Controls an entire origin (domain) or specific paths within it.
- **Requirement:** Requires HTTPS (except for `localhost`).
- **Cannot** access the DOM.
- **(Note):** While technically a worker, its role is vastly different from a Dedicated or Shared Worker.

**Worklets:**

- A low-level API that gives developers access to the browser's rendering pipeline.
- Allows running JS code on the render thread (e.g., for custom paint or audio processing effects).
- Very specialized and not for general-purpose background computation.

## Link to read

https://dev.to/helmuthdu/web-workers-the-unsung-heroes-of-responsive-web-apps-3j78

https://dev.to/mino/unlocking-performance-a-comprehensive-guide-to-web-workers-25i7

https://github.com/ZeeshanAli-0704/front-end-system-design/blob/main/Concepts/Web_Worker_SharedWorker_and_Service_Worker.md
