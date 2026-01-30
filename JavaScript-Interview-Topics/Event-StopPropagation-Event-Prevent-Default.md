# Event StopPropagation & Event PreventDefault

- Both are methods on the Event object, but they do DIFFERENT things.

# 1) event.stopPropagation()

- Stops the event from traveling further (no bubbling/capturing).
- Use it when you don’t want parent/ancestor elements to handle the event.

```js
// Example HTML:
<div id="parent">
  <button id="child">Click Me</button>
</div>;

let parent1 = document.getElementById("parent");
let child1 = document.getElementById("child");

parent1.addEventListener("click", () => {
  console.log("Parent clicked!");
});

child1.addEventListener("click", (e) => {
  e.stopPropagation(); // Stops bubbling
  console.log("Child clicked only!");
});

// Output when clicking button:
// "Child clicked only!"
// (Parent listener does NOT fire because bubbling is stopped)
```

# 2) event.preventDefault()

- Prevents the DEFAULT browser action of an element (but event still bubbles).
- Example: stop link navigation, stop form submission, etc.

```js
// Example HTML:

<a id="link" href="https://google.com">
  Go to Google
</a>;

let link = document.getElementById("link");

link.addEventListener("click", (e) => {
  e.preventDefault(); // Prevents browser navigation
  console.log("Default action prevented. Staying on same page!");
});

// Clicking link → does NOT redirect, but still logs message.
```

# 3) Using Both Together

- Sometimes you need BOTH: prevent default + stop bubbling.

```js
child.addEventListener("click", (e) => {
  e.preventDefault();
  e.stopPropagation();
  console.log("No default + no bubbling!");
});
```

✅ Q&A Section

**Q1) What does event.stopPropagation() do?**

Stops event from traveling further (no bubbling/capturing).

**Q2) What does event.preventDefault() do?**

Stops the element’s default browser action (like link navigation, form submit).

**Q3) Do they do the same thing?**

No. One controls event flow, the other controls default browser behavior.

**Q4) Can I use both together?**

Yes, if you want to stop browser’s default AND stop event flow.

**Q5) Example use case of preventDefault?**

Prevent a form from submitting when validation fails.

**Q6) Example use case of stopPropagation?**

Button inside a modal → prevent click from also closing modal via parent listener.

**✅ Recap**

- stopPropagation → stop event from reaching parents
- preventDefault → stop browser’s default action
- They are DIFFERENT, but can be used together
