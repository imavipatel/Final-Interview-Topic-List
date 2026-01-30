# 🌐 Web API in JavaScript — Short Notes

## What is a Web API?

- **Web APIs** are **features provided by the browser**, not JavaScript itself.
- They allow JavaScript to **interact with the browser and the web page**.
- JavaScript uses Web APIs to do things it **cannot do alone** (like DOM access, timers, HTTP requests).

📌 **Important:**  
JavaScript = language  
Web API = browser powers

---

## Why Web APIs are needed?

- Manipulate HTML & CSS
- Handle user actions (click, scroll, input)
- Fetch data from servers
- Store data in the browser
- Work with time, media, location, etc.

---

## Common Web APIs

### 1️⃣ DOM API

- Used to access and modify HTML elements

```js
document.getElementById("title").textContent = "Hello";
```

# Fetch API

- Used to make HTTP requests (API calls)

```js
fetch("/api/data")
  .then((res) => res.json())
  .then((data) => console.log(data));
```

# Timer API

- Execute code after a delay or repeatedly

```js
setTimeout(() => {
  console.log("Hello");
}, 1000);

setInterval(() => {
  console.log("Hello");
}, 1000);
```

# Storage API

- Store data in browser

```js
localStorage.setItem("name", "Avi");
```

# Console API

- Debugging

```js
console.log("Debug message");
```

# Web API Execution Flow

JS Code → Web API → Callback Queue → Event Loop → Call Stack

# Key Points to Remember

- Web APIs are asynchronous

- They are provided by browsers, not ECMAScript

- Node.js has different APIs, not Web APIs

# Examples of Web APIs

- DOM

- Fetch

- Geolocation

- Canvas

- Web Storage

- Web Workers

**Web APIs let JavaScript talk to the browser and the outside world.**
