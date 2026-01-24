### Tree Shaking

As JavaScript projects grow, bundle sizes tend to explode. That means slower load times, higher bandwidth usage, and frustrated users.

**Tree Shaking** is the process of removing unused or “dead” code during the build step.
If a function, variable, or module isn’t actually used anywhere, it gets excluded from the final bundle.
It’s called “tree shaking” because the build tool shakes the dependency tree and drops dead leaves 🍃.

**Why Should You Care?**
I get it—you might think: "My app works fine, who cares about a few extra kilobytes?" But here's the thing:

**1. Reduced Bundle Size => Smaller bundles = Faster downloads.** Think about someone on a 4G connection downloading your app. Every KB matters. A 500KB bundle vs. a 300KB bundle is literally 40% extra data transferred. That's noticeable. That's waiting. That's users closing your tab and going to a competitor.

**2. Improved Load Times => Performance matters for business.** Smaller bundles mean faster load times for your web application. When less JavaScript code needs to be downloaded, parsed, and executed by the browser, the application becomes more responsive

Studies show that every 100ms delay in load time costs you users. Tree shaking isn't magic, but it helps.

**3.Optimized Resource Usage**
Tree shaking helps reduce the amount of unnecessary code being executed by the browser. This can lead to lower memory consumption, which is especially important for devices with limited resources, such as mobile phones.

**It's free optimization.** You don't have to rewrite your code. Your bundler does the work for you in production.

## Things to remember=>

**1. Avoid using common js**

```js
exports.add = (a, b) => a + b; ✅ Used
exports.multiply = (a, b) => a \* b; ❌ Dead code
exports.subtract = (a, b) => a - b; ❌ Dead code
exports.divide = (a, b) => a / b; ❌ Dead code

const utils = require('./utils');
console.log(utils.add(5, 3)); // Only add!
```

Here's the problem: they only used add. But bundler still included everything! Why?

Because CommonJS is dynamic. it include everything

**2. Use ES6 import export module strategy**

ES Modules (ESM). Unlike CommonJS, ESM allows static analysis of the code, making it possible for bundlers like **Webpack, Rollup**, and others to identify which parts of the code are actually used and which parts can be safely removed.

```js
// math.js (ES6)
export const add = (a, b) => a + b;
export const multiply = (a, b) => a \* b;
export const subtract = (a, b) => a - b;
export const divide = (a, b) => a / b;

// app.js
import { add } from './math';
console.log(add(5, 3));
```

The bundler sees: "Only add is imported. I can safely remove the others."

**3. Avoid The import \* Mistake (Don't Do This)**

It's like showing up to a party and inviting everyone on the guest list, even though you only know three people.

```js
import * as math from "./math";
console.log(math.add(5, 3));

// Without Tree Shaking
import * as react from "react";
// Bundle 500KB
```

import in this way

```js
// With Tree Shaking
import { useState } from "react";
// Bundle 120KB
```

When you import everything with \*, you're basically telling the bundler: "I need ALL of these exports, I promise!" So tree shaking throws its hands up and says, "Fine, I'll include everything."

**4. Don’t Export Destructured Variables**

```js
const config = {
  API_URL: "...",
  TIMEOUT: 3000,
};

// ❌ Not tree-shakable
export const { API_URL, TIMEOUT } = config;

// ✅ Better
export const API_URL = "...";
export const TIMEOUT = 3000;
```

**5. Use Named Exports Instead of default**

Named exports make it easier for bundlers to remove unused code.

```js
// ❌ Not tree-shakable
export default {
  add,
  subtract,
};

// ✅ Tree-shakable
export const add = () => {};
export const subtract = () => {};
```

Tree shakers like Rollup and Webpack can’t shake off unused properties of a default-exported object.

**Side Effects Are Tricky (This One Caught Me Off Guard)**
Here's something that catches many developers off guard. Consider a module that looks like this:

```js
// logger.js
console.log("Logger started!"); // <-- Side effect!

export const log = (msg) => console.log(msg);
export const warn = (msg) => console.warn(msg);
export const error = (msg) => console.error(msg);
```

That console.log at the top? That's a side effect. It runs when the module loads, no matter what you import from it.

Now when you import just log:

```js
import { log } from "./logger";
log("Hello world");
```

The bundler thought: "I can't remove this module because it has a side effect. That console.log might be critical!"

So even though you don't use warn or error, they stayed in the bundle.

The fix? Move the side effect into a function:

```js
// logger.js (Fixed!)
const initLogger = () => {
  console.log("Logger started!");
};

export const log = (msg) => console.log(msg);
export const warn = (msg) => console.warn(msg);
export const error = (msg) => console.error(msg);

// Call this only when you need it
initLogger();
```

Now if you don't import warn or error, they get removed. No side effects blocking the way! 🎯

### ⚙️ Use a Bundler that Supports Tree Shaking

To take full advantage of tree shaking, you need to use a bundler that supports the technique. Popular bundlers like **Webpack, Rollup, and Parcel** support tree shaking out-of-the-box.

Tree shaking works only with ES modules (ESM) that means you must use import and export, not require.

Here’s a minimal Webpack configuration that supports it:

```js
// webpack.config.js
module.exports = {
  mode: "production", // Important: enables minification and tree shaking
  entry: "./src/main.js",
  output: {
    filename: "bundle.js",
    path: __dirname + "/dist",
  },
  optimization: {
    usedExports: true, // Marks used exports for tree shaking
    sideEffects: false,
  },
};
```

## Optimization

**usedExports**

The usedExports property is responsible for marking the unused functions with brown leaves.

That’s it! Once you build your project with webpack --mode production, unused code is automatically removed.

The main purpose of manually configuring sideEffects is to eliminate unused reexported modules within barrel files, resulting in a significant reduction in bundle size, improving performance and loading times.

## Tree Shaking

During the build process, Webpack creates a dependency tree structure. In the tree, each dependency (module) is represented by a branch and each function is represented by a leaf. There are green branches (which represent modules with used functions or with side effects), light green leaves (which represent imported used functions), brown leaves (which represent unused functions from imported module, also known as "dead code") and dark brown branches (which represent imported unused modules with no side effects). When we shake the tree, the brown leaves should fall off while the green leaves remain. In our case, this means that after tree shaking only imported used functions or modules with used functions or side effects should remain.

**Link to read**
https://dev.to/fogel/tree-shaking-in-webpack-5apj

https://dev.to/text/tree-shaking-for-javascript-library-authors-4lb0

https://javascript.plainenglish.io/deep-dive-into-tree-shaking-ba2e648b8dcb
