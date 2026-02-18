# Debouncing & Throttling

**📌 Why needed?**

- In JavaScript, events like typing, scrolling, resizing, or clicking can fire many times in a short period.
- This may cause performance issues (too many function calls).
- To control this, we use: Debouncing & Throttling.

# Debouncing

- Debouncing is a programming technique that helps to **improve performance** of an application **by limiting frequency of function calls**.

- Delaying the execution of function calls until **certain time has passed** since **last time it was called.**

- Har last call ke baad time ko reset krte hai

**Use Cases**

- validate form
- autosave draft when user stop editing
- search box,
- autocomplete
- resize events.

```js
// Example: Debouncing
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer); // reset timer every time function is called
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}

function searchQuery(query) {
  console.log("🔍 Searching for:", query);
}

// Wrap the search function with debounce
const debouncedSearch = debounce(searchQuery, 500);

// Simulating user typing
debouncedSearch("J");
debouncedSearch("Ja");
debouncedSearch("Jav");
debouncedSearch("Java");
// Only the last call ("Java") will execute after 500ms delay
```

# 2) THROTTLING

- Execute function at regular intervals, no matter how many times triggered"

- Its enusres that function is called at most once within specified amount of time interval.
- Function runs periodically instead of every event

- **Use Cases:**

- scroll events
- window resize
- button clicks.

```js
function throttle(fn, limit) {
  let inThrottle;
  return function (...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

function logScroll() {
  console.log("📜 Scroll event at", new Date().toLocaleTimeString());
}

const throttledScroll = throttle(logScroll, 1000);

// Simulating scroll events (fires every 50ms)
let interval = setInterval(throttledScroll, 50);
setTimeout(() => clearInterval(interval), 4000);

// Output: Will log only once per second, not 80 times!
```

# Interview Q&A

**Q1) What is Debouncing?**
A technique to call a function only after the user stops performing an action.

**Q2) What is Throttling?**
A technique to limit how often a function runs in a given time frame.

**Q3) Real-life examples?**

Debounce: Search suggestions should show only after typing stops.

Throttle: Scroll handler should update position once every 200ms.

**Q4) Which improves performance more?**
Both do, depending on use case:

- Debounce = wait for pause
- Throttle = run at fixed pace

**Q5) Can you use both together?**
Yes, sometimes combined for maximum efficiency.

**Debounce → "Wait until done"** (e.g., typing search)
**Throttle → "Do at regular pace"**(e.g., scroll)

https://sugandsingh5566.medium.com/mastering-throttle-and-debounce-functions-in-react-native-with-javascript-a03965240829
