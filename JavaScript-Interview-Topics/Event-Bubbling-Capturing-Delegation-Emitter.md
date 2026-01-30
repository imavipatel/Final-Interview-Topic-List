# Event Handling: Bubbling, Capturing, Delegation, Emitter

# What are events?

- Events are actions that happen in the browser (click, keypress, scroll, etc.).
- We use JavaScript to "listen" and "react" to them using event listeners.

```js
// Example HTML (for explanation):
<div id="parent">
  <button id="child">Click Me</button>
</div>
```

# What is Event Handling?

- Event handling is the process of writing JavaScript code that runs when an event occurs.

- It defines how your application reacts to user actions.

**🔹 Example Explanation:**

**When a user clicks a button →**

- JavaScript executes a function →

- An action is performed (show message, submit form, etc.)

- Event → Handler → Action

# What Is Event Propagation?

In JavaScript, when an event happens (like a click), it doesn’t just happen on that element — it travels through the DOM. There are two main phases:

# 1) Event Bubbling

- By default, events move UP from the target element to its parents.

- Think of it like bubbles in water - once they are created, they rise to the top.

- Example: Clicking button → fires on button → then parent → then body → etc.

```js
<div id="parent">
  <button id="child">Click Me</button>
</div>;

let child = document.getElementById("child");
let parent = document.getElementById("parent");

child.addEventListener("click", () => {
  console.log("Button clicked!"); // fires first
});

parent.addEventListener("click", () => {
  console.log("Parent div clicked!"); // fires second
});
//
// Order: Button → Parent (bubbling up)
//
```

# 2) Event Capturing (Trickling)

- Opposite of bubbling: event moves DOWN from root (parent) to → target.

- To enable capturing, pass { capture: true } in addEventListener.

```js
<div id="parent">
  <button id="child">Click Me</button>
</div>;

let child = document.getElementById("child");
let parent = document.getElementById("parent");

parent.addEventListener(
  "click",
  () => {
    console.log("Parent capturing!");
  },
  { capture: true },
);

child.addEventListener("click", () => {
  console.log("Child clicked!");
});

// Order with capturing: Parent (capturing) → Child
```

# Example 2

```js
const parent = document.getElementById("parent");
const child = document.getElementById("child");

parent.addEventListener(
  "click",
  () => {
    console.log("Parent capturing phase");
  },
  true,
); // Capturing phase

child.addEventListener("click", () => {
  console.log("Child bubbling phase");
}); // Bubbling phase
```

# 3) Event Delegation

- Instead of adding event listeners to many children,
- we add ONE listener to the parent and check the target.
- Useful when dynamically adding/removing elements.

```js
parent.addEventListener("click", (e) => {
  if (e.target && e.target.id === "child") {
    console.log("Delegated: Button clicked!");
  }
});
```

**Advantage:**

- Better performance (fewer listeners),
- works for dynamically added elements.

# 4) Event Emitter (Custom Events)

- Custom event emitters allow developers to create, dispatch, and listen to their own events for better modularity.

```js
const eventEmitter = {
  events: {},
  on(event, listener) {
    if (!this.events[event]) this.events[event] = [];
    this.events[event].push(listener);
  },
  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach((listener) => listener(data));
    }
  },
};

eventEmitter.on("dataReceived", (data) => {
  console.log("Data received:", data);
});

eventEmitter.emit("dataReceived", { id: 1, message: "Hello!" });

// Example 2

// Example: custom "hello" event
let myDiv = document.createElement("div");

myDiv.addEventListener("hello", (e) => {
  console.log("Hello event received:", e.detail);
});

// Dispatch custom event
let event1 = new CustomEvent("hello", { detail: { name: "Avi" } });
myDiv.dispatchEvent(event1);
// Output: Hello event received: { name: "Avi" }
```

# 4). Event Once Handling

**What is it?**

- Sometimes you only want an event handler to execute once. Modern JavaScript provides an elegant way to handle this.

```js
const button = document.getElementById("myButton");

button.addEventListener(
  "click",
  () => {
    console.log("Button clicked!");
  },
  { once: true },
);
```

**Why use it?**

- Simplifies logic for one-time events.
- Avoids memory leaks by automatically removing the listener.

# ✅ Q&A Section

**Q1) What is event bubbling?**
Event goes from child → parent → ancestors.

**Q2) What is event capturing?**
Event goes from parent → child(top to bottom).

**Q3) Which phase runs by default?**
Bubbling phase (unless you set { capture: true }).

**Q4) What is event delegation and why use it?**

Attach one listener on parent → handle many children.
Benefits: performance + dynamic element handling.

**Q5) What is an Event Emitter?**
A way to create and trigger custom events in JS.

**Q6) How to stop bubbling?**
Use event.stopPropagation().

**Example:**

```js
child.addEventListener("click", (e) => {
  e.stopPropagation(); //Prevents event going to parent
});
```
