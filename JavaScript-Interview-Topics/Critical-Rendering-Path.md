# Critical Rendering Path

**What is Critical Rendering Path?**

- Critical Rendering Path is the **sequence of steps** the browser takes to **convert HTML, CSS, and JavaScript** into pixels on the screen.

- It’s called “critical” because any delay in this path directly delays the first paint — i.e., how fast users see something.

- Basically: "How does your code become a web page that you can see and use?"

- So, **optimizing the CRP = optimizing perceived performance** (Time to First Paint, Time to Interactive, etc.).

# How DOM is created?

**DOM construction is incremental**

- HTML response turn into tokens which turn into nodes. which turns into DOM Tree

**Steps in the Critical Rendering Path**

**Step 1: HTML Parsing → DOM**

- Browser downloads and parse HTML to build the DOM Tree.
- HTML requests JS which again alters DOM.
- Example:

```html
<p>Hello</p>
```

becomes a DOM node.

**Step 2: CSS Parsing → CSSOM**:

- Browser downloads and parses all CSS files (inline + external) to build **CSSOM (CSS Object Model).**

- Example: p { color: red } applies to <p> node.

**Step 3: Combine DOM + CSSOM → Render Tree**:

- Browser engine combines these two DOM & CSSOM to create Render Tree.
- Browser merges DOM and CSSOM to know what each node looks like.
- Each node now knows what to paint (color, size, position, etc.).
- Example:

```html
<p>Hello</p>
```

styled with red → "Render Tree node: red text 'Hello'".

**Step 4: Layout (Reflow)**:

- Browser (Reflow) Layout determine position and size of each render tree node. (The geometry).

**Step 5: Paint (Rasterization)**:

- Once layout is determined. Browser paints pixels on the screen (colors, borders, text, etc.).

**Step 6: Composite**:

- Layers (like z-index, transforms) are combined and shown to the user.

# Example Flow

**HTML:**

```html
<html>
  <head>
    <style>
      p {
        color: blue;
      }
    </style>
  </head>
  <body>
    <p>Hello World</p>
  </body>
</html>
```

# CRP steps:

- HTML parsed → DOM: <p>Hello World</p>
- CSS parsed → CSSOM: p { color: blue }
- DOM + CSSOM → Render Tree
- Layout: p positioned at (x, y) with width/height
- Paint: text "Hello World" painted in blue

# Render-blocking resources

**Some resources stop CRP until they’re ready:**

- **CSS**: page can’t render until CSSOM is built.
- **JS (without async/defer)**: can block DOM parsing.
- **Solution:** use

```js
<script defer> <script async>
```

minify, preload.

# Optimizations (CRP Performance Tips)

- **Remove unused CSS** (via PurgeCSS, CSS Tree-shaking)
- **Lazy-load JS modules** not needed at start
- **Defer non-critical JS**

```js
<script defer> or <script async>
```

- **Use resource hints:**

```html
<link rel="preload" />
<link rel="prefetch" />
<link rel="dns-prefetch" />
```

# Visual Diagram (Text Version)

- HTML -----> DOM
- CSS ------> CSSOM
- DOM + CSSOM -----> Render Tree
- Render Tree -----> Layout -----> Paint -----> Screen

# Interview Q&A

**Q1) What is the Critical Rendering Path?**
The sequence of steps browser takes to render pixels on the screen:
HTML → DOM → CSSOM → Render Tree → Layout → Paint → Composite.

**Q2) Why is CSS render-blocking?**
Because without CSS, the browser doesn’t know how elements should look,
so it waits for CSSOM before painting.

**Q3) How to make JavaScript non-blocking?**
Use "async" or "defer" attributes when loading scripts.

**Q4) Difference between DOM and Render Tree?**
DOM = structure of the page; Render Tree = DOM + CSSOM with visible nodes only.

**Q5) How to optimize CRP for faster page load?**
Reduce blocking resources, use critical CSS, defer JS, lazy load images.

# Quick Recap (Easy to Remember)

**CRP steps:**

1. HTML → DOM
2. DOM + CSSOM → Render Tree
3. Layout (positions/sizes)
4. Paint (pixels on screen)
5. Composite (final display)

**Performance tips:**

- Defer/async JS
- Minimize CSS/JS
- Use critical CSS
- Lazy load non-critical assets

# 🧩 Example: Before vs After Optimization

**❌ Before:**

```html
<head>
  <link rel="stylesheet" href="main.css" />
  <script src="jquery.js"></script>
  <script src="analytics.js"></script>
</head>
<body>
  <header>Welcome</header>
  <main>...</main>
</body>
```

# Step-by-Step Breakdown (Before Optimization)

**HTML Download Starts**

- Browser starts downloading HTML from the server.
- As it parses the HTML, it encounters resources

```html
<link> and <script>.
```

**Encounter**

```html
<link rel="stylesheet" href="main.css" />
```

- CSS files are render-blocking.
- The browser pauses rendering until the CSS file is fully downloaded and parsed (to build the CSSOM).

**⛔ Why?**
Because the browser must know what elements look like before painting anything on screen.

**Encounter**

```js
<script src="jquery.js"></script>
```

- JavaScript files (without defer or async) are parser-blocking.
- HTML parsing stops completely until jquery.js is downloaded and executed.

**⛔ Why?**
Because JS can modify the DOM or CSSOM dynamically (e.g., _document.write(), style changes_), so the browser can’t safely continue building the DOM until the JS finishes.

**Encounter**

```js
<script src="analytics.js"></script>
```

- Same issue: blocks HTML parsing again.
- Even though analytics doesn’t affect rendering, it still delays everything.

**Browser Builds:**

- DOM Tree (after parsing resumes)
- CSSOM Tree (after CSS downloaded)
- Render Tree (combining both)

Only after both **DOM + CSSOM** are ready does the **render tree** form and **painting starts**.

⏳ So user waits unnecessarily long — even for **non-critical scripts** like analytics.

**Measured Performance Impact (Before)**
**Metric What Happens Impact**

- DOM Parsing - Blocked by JS - Slow
- CSSOM Building - Blocks render - Delayed FCP
- JavaScript Execution - Blocks DOM - Delayed TTI
- Network Requests - Sequential - More round trips
- Perceived Load Time - Blank screen longer - Poor UX

**✅ After:**

```html
head>
  <!-- 1️⃣ Inline only critical CSS -->
  <style>
    header { background: #fff; font-family: sans-serif; }
  </style>

  <!-- 2️⃣ Preload & load non-critical CSS async -->
  <link rel="preload" href="main.css" as="style" onload="this.rel='stylesheet'">

  <!-- 3️⃣ Load app logic after parsing -->
  <script src="main.js" defer></script>

  <!-- 4️⃣ Load non-critical JS async -->
  <script src="analytics.js" async></script>
</head>
<body>
  <header>Welcome</header>
  <main>...</main>
</body>
```

# Step-by-Step Breakdown (After Optimization)

**Inline Critical CSS**

- Inlines just enough styles for the **above-the-fold content** (header, layout basics).
- Browser can **start painting immediately**, even before downloading external CSS.
  🎯 This directly reduces Time to First Paint (FP) and First Contentful Paint (FCP).

**Load Non-Critical CSS Asynchronously**

- **preload** hints the browser: “fetch this early” (high priority).
- But rendering **doesn’t block** because it’s not a render-blocking stylesheet yet.
- After it loads, the onload handler changes rel to stylesheet, applying the CSS.
  **⚡ Result:**

- Browser starts downloading main.css early, but doesn’t block first paint.

**Defer Main JS Logic**

```html
<script src="main.js" defer></script>
```

- defer downloads the script i**n parallel** with HTML parsing.
- It executes **only after the DOM is fully built**.
- Doesn’t block DOM construction.
  **⚡ Result:**

- Faster parsing and earlier rendering.
- JS logic still runs at the right time.

**Async for Non-Critical JS**

```js
<script src="analytics.js" async></script>
```

- async downloads the script **in parallel** and executes it **immediately after downloading**.
- Doesn’t block DOM parsing.
- Best for **independent scripts** (e.g., analytics, ads, metrics).

**⚡ Result:**

- Analytics loads fast, but doesn’t delay rendering.

**What Happens Now (Optimized Flow)**

1. Browser downloads HTML → starts parsing immediately.
2. Inline CSS available instantly → early paint possible.
3. main.css fetched asynchronously (won’t block rendering).
4. DOM parsing continues uninterrupted.
5. JS files (main.js, analytics.js) downloaded in parallel.
6. main.js runs after DOM ready; analytics.js runs whenever ready.
7. User sees page much earlier.

# Measured Performance Impact (After)

**Metric** **What Happens** **Impact**

DOM Parsing - Non-blocked - ✅ Faster
CSSOM Building - Critical inline + async CSS ✅ - Parallelized
JS Execution - Deferred / async - ✅ Non-blocking
Network Requests - Parallel - ✅ Fewer delays
Perceived Load Time - Early paint possible - ✅ Great UX
Core Web Vitals (FCP/LCP) - Improved ✅ - Significant gain

**Link to read**
https://dev.to/zeeshanali0704/frontend-system-design-what-is-the-critical-rendering-path-crp-11bm

https://dev.to/abhay1kumar/understanding-web-storage-localstorage-sessionstorage-and-cookies-1384
