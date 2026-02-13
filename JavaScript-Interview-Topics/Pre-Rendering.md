# PRE-RENDERING

**What is Pre-Rendering?**

Pre-rendering means:

- HTML is generated before it reaches the browser
- So user sees content immediately

Instead of:

```js
Empty HTML → JS Loads → UI Appears
```

We send:

```js
Ready HTML → JS Hydrates → Interactive Page
```

# Why Pre-Rendering?\*\*

**Problems with normal Client-Side Rendering (CSR):**

- Blank screen initially
- Slow first load
- Bad SEO

**Pre-rendering fixes:**

✔ Faster first paint
✔ Better SEO
✔ Better performance

# Types of Pre-Rendering

**1️⃣ Static Site Generation (SSG)**

👉 HTML created at build time
👉 Same for every user

Used in:

Next.js
Gatsby

**2️⃣ Server-Side Rendering (SSR)**

👉 HTML created on every request
👉 Dynamic per user

Used in:

Next.js
Angular Universal

**Example – CSR vs Pre-render**

**Without Pre-render**

```js
<body>
  <div id="root"></div>
  <script src="bundle.js"></script>
</body>
```

User sees: ⚪ Blank screen

**With Pre-render**

```js
<body>
  <div id="root">
    <h1>Hello Avi</h1>
  </div>
  <script src="bundle.js"></script>
</body>
```

User sees: ✅ Content immediately

**🔹 Hydration**

After pre-rendered HTML loads:

React attaches event listeners
This is called Hydration

```js
import { hydrateRoot } from "react-dom/client";

hydrateRoot(document.getElementById("root"), <App />);
```

**Interview One-Line Definition**

Pre-rendering means generating HTML before sending it to the browser to improve performance and SEO.
