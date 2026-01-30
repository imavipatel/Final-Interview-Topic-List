# MutationObserver vs IntersectionObserver

**Theory**

Both MutationObserver and IntersectionObserver are browser APIs
They help observe changes without continuous polling
Used mainly for performance optimization

# MutationObserver

- Keep checking whether there is any mutation in DOM tree or not.

# IntersectionObserver

- It monitors when an element enters into the viewport

**MutationObserver**:

-A **MutationObserver** is a built-in JavaScript Web API that Keep checking whether there is any mutation in DOM tree or not.

-It provides an efficient, asynchronous way to detect modifications such as the addition or removal of elements, changes to an element's attributes, or modifications to the text content within nodes.

**Key Features and Benefits**

**Asynchronous Execution:** MutationObserver callbacks run on the **microtask queue**, which means they are highly responsive and **do not block the browser's main rendering thread** or impact user interaction performance.

**Batching of Changes:** Instead of firing an event for every single change (which caused performance issues with the deprecated Mutation Events), observers collect all DOM updates related to a single repaint and provide them in an array to the callback function

**Granularity:** Developers can specify precisely which types of changes to observe (e.g., only childList changes, attributes, or characterData), and whether to extend observation to an element's entire subtree

# Common Use Cases

- Detect the presence of specific elements (e.g.; add banners ) and take action if they are removed by ad blockers.
- detect any type of DOM modifications
- Used to implement auto-save functionality in online code editors (e.g.'; Leetcode, Hackerrank, GFG, etc).
- Detect UI updates of multiple elements on real real-time basis.
- Monitor input field for changes and validate user input in real-time, showing them error messages and feedback if required.

# Key API:

- new MutationObserver(callback)
- observer.observe(targetNode, options)
- observer.disconnect()
- callback receives (mutationsList, observer) — an array of MutationRecord

# MutationRecord important fields:

- type: "childList" | "attributes" | "characterData"
- target: node where change was observed
- addedNodes, removedNodes (NodeLists)
- attributeName (if attributes changed)
- oldValue (if requested via option)

**Example: Observe child nodes and attribute changes**

```js
const targetNode = document.getElementById("list"); // assume <ul id="list"></ul>

const observerConfig = {
  childList: true, // watch for added/removed children
  attributes: true, // watch for attribute changes
  subtree: false, // set true to watch descendants too
  attributeOldValue: true, // record previous attribute value
  characterData: false, // watch text changes (set true if needed)
};

const mutationCallback = (mutationsList, observer) => {
  for (const mutation of mutationsList) {
    if (mutation.type === "childList") {
      if (mutation.addedNodes.length) {
        console.log("Nodes added:", mutation.addedNodes);
      }
      if (mutation.removedNodes.length) {
        console.log("Nodes removed:", mutation.removedNodes);
      }
    } else if (mutation.type === "attributes") {
      console.log(
        `Attribute "${mutation.attributeName}" changed on`,
        mutation.target,
        "old value:",
        mutation.oldValue,
      );
    } else if (mutation.type === "characterData") {
      console.log(
        "Text changed in",
        mutation.target,
        "new value:",
        mutation.target.data,
      );
    }
  }
};

const mo = new MutationObserver(mutationCallback);
mo.observe(targetNode, observerConfig);

// Trigger some mutations (demo)
const li = document.createElement("li");
li.textContent = "Item X";
targetNode.appendChild(li); // -> childList mutation
li.setAttribute("data-active", "1"); // -> attributes mutation
li.removeAttribute("data-active"); // -> attributes mutation
targetNode.removeChild(li); // -> childList mutation

// Stop observing when done
// mo.disconnect();
```

# Best practices (MutationObserver):

- Narrow scope: observe a specific subtree, not document.body (unless necessary).
- Use attributeFilter to only watch certain attributes.
- Heavy work inside callback? Batch or debounce to avoid layout thrashing.
- Disconnect observers when no longer needed to avoid memory leaks.

# IntersectionObserver

- It monitors when an element enters into the viewport

- Watches when an element becomes visible (or passes a visibility threshold) relative to a root (viewport by default). Think: "Is this element on screen?"

- It provides a high-performance, asynchronous way to detect when a target element enters or exits view port.

- It is widely used for performance-related tasks

# Common use cases

**Lazy Loading:** Loading images or other content only when they are about to enter the user's viewport, which conserves network resources and improves initial page load performance.
**Infinite Scrolling:** Automatically loading more content as the user approaches the bottom of the page.
**Scroll-Based Animations:** Triggering animations or performing tasks based on whether an element is in view.
**Ad Visibility Reporting:** Determining the visibility of advertisements to calculate ad impressions and revenue.

# Key API:

- new IntersectionObserver(callback, options)
- observer.observe(element)
- observer.unobserve(element)
- observer.disconnect()

**Key Concepts**

**Target:** The element being observed for intersection changes.
**Root:** The element or the document's viewport (default) that the target is intersecting with. The target must be a descendant of the root.
**Intersection Ratio:** A value between 0.0 and 1.0 indicating the percentage of the target element that is visible within the root's intersection rectangle.
**Threshold:** A single number or an array of numbers that defines the percentage(s) of visibility at which the observer's callback function should be executed. A default of 0 means even a single pixel triggers the callback, and 1.0 means the entire element must be visible.
**rootMargin:** Similar to CSS margins, this property allows the developer to grow or shrink the root's bounding box, effectively creating an offset for when the intersection is calculated.

**Options: { root, rootMargin, threshold }**

- root: element used as viewport. null => browser viewport.
- rootMargin: CSS margins (e.g. "0px 0px 200px 0px") — lets you trigger earlier.
- threshold: number or array [0, 0.25, 1] — proportion of target visible to trigger.

**Callback receives (entries, observer). Each entry is an IntersectionObserverEntry:**

- entry.target (the element)
- entry.isIntersecting (true if it intersects above threshold)
- entry.intersectionRatio (0.0 to 1.0)
- entry.boundingClientRect / intersectionRect (geometry info)

```js
// Example 1: Lazy-load images
// HTML: <img data-src="big.jpg" width="600" height="400" alt="...">
const lazyImages = document.querySelectorAll("img[data-src]");

const lazyLoad = (entries, observer) => {
  for (const entry of entries) {
    if (entry.isIntersecting) {
      // element is visible (per threshold)
      const img = entry.target;
      img.src = img.dataset.src; // start loading the real image
      img.removeAttribute("data-src"); // mark as loaded
      observer.unobserve(img); // no longer need to watch this image
    }
  }
};

const io = new IntersectionObserver(lazyLoad, {
  root: null, // viewport
  rootMargin: "0px 0px 200px 0px", // start loading 200px before it appears
  threshold: 0, // 0 means any pixel visible triggers it
});

lazyImages.forEach((img) => io.observe(img));

// Example 2: Infinite scroll sentinel
// HTML: <div id="sentinel"></div>
const sentinel = document.getElementById("sentinel");

const loadMore = async () => {
  // fetch or generate more DOM nodes, append to list
  console.log("Load more items...");
  // after appending items you may need to move the sentinel
};

const sentinelObserver = new IntersectionObserver(
  (entries) => {
    if (entries[0].isIntersecting) {
      sentinelObserver.unobserve(sentinel); // prevent double triggers while loading
      loadMore().then(() => sentinelObserver.observe(sentinel));
    }
  },
  { root: null, threshold: 0.1 },
);

sentinelObserver.observe(sentinel);
```

**Best practices (IntersectionObserver):**

- Use rootMargin to prefetch (load before element fully appears).
- Use unobserve or disconnect when done to save resources.
- Choose threshold carefully (0 for any intersection, 1 for fully visible).
- Avoid observing huge numbers of elements individually — consider grouping or using pagination if thousands of nodes are involved.

# Common pitfalls & simple fixes

Pitfall: Observing entire document with MutationObserver and doing expensive work

Fix: Observe a narrow subtree, use attributeFilter, and debounce updates.

Pitfall: IntersectionObserver firing many times for many small elements

Fix: Use sensible threshold and unobserve elements once handled.

## Q&A — quick revision (simple answers)

**Q1) What does MutationObserver do?**
Watches DOM changes: nodes added/removed, attributes changed, text changed.

**Q2) What does IntersectionObserver do?**
Watches element visibility relative to a root (e.g., viewport).

**Q3) How do you stop an observer?**
Use observer.disconnect() to stop everything, or observer.unobserve(node) to stop a single node.

**Q4) How to get old attribute value with MutationObserver?**

Set option attributeOldValue: true when observing — then mutation.oldValue is available.

**Q5) Can IntersectionObserver be used to lazy-load images?**
Yes — observe images with data-src; when they intersect, set src and unobserve.

**Q6) Should observer callbacks be heavy?**
No — keep callbacks light, batch work, and avoid forcing layouts inside the loop.

**Q7) What is threshold in IntersectionObserver?**
A fraction (0-1) or array that determines how much of the target must be visible before the callback fires (e.g., 0.5 means 50% visible).

**Q8) What is rootMargin?**

Margin around the root bounding box (like CSS margin) to expand/shrink the root for earlier/later triggering (useful for prefetching).

**Q9) Do observers run synchronously as soon as a change occurs?**

No — the browser batches changes and calls the callback asynchronously with arrays of changes. This reduces thrashing but means you should write your callback accordingly.

**Q10) How to avoid memory leaks?**
Disconnect observers when element/component is removed (e.g., in component cleanup).

# Quick Recap

- Use MutationObserver when you need to know that the DOM changed.
- Use IntersectionObserver when you need to know that an element is visible.
- Both are powerful, efficient alternatives to polling and manual checks.
- Keep callbacks small, batch updates, and disconnect when finished.

https://dev.to/shailseen/why-intersection-observer--1gnj
