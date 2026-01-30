# Local Storage vs Session Storage

**📝 Theory**

- Both `localStorage` and `sessionStorage` are part of the **Web Storage API**.
- They are used to store key–value pairs in the browser, like a mini database.

**✅ Similarities:**

- Both store data in browser
- Data stored as **string key-value pairs**
- More storage (5–10 MB) than cookies (~4KB)
- Cannot send data automatically to the server (unlike cookies)
- Accessed using: setItem, getItem, removeItem, clear

**1) localStorage**

- Data is saved permanently in the browser (until manually cleared).
- Survives page refresh & browser restart.
- Good for storing user preferences, theme, cart data.

```js
// Example:
localStorage.setItem("theme", "dark"); // Save data
console.log(localStorage.getItem("theme")); // "dark"
localStorage.removeItem("theme"); // Remove one
localStorage.clear(); // Clear all
```

- Even if you close browser and open again → data is still there! 🔒

**2) sessionStorage**

- Data exists only for the current **tab/session**.
- Cleared automatically when the tab is closed.
- Good for temporary data (form inputs, one-time session).

```js
// Example:
sessionStorage.setItem("username", "Avi"); // Save data
console.log(sessionStorage.getItem("username")); // "Avi"
sessionStorage.removeItem("username"); // Remove one
sessionStorage.clear(); // Clear all
```

- If you refresh the tab → data is there.
- But if you close the tab/browser → data is gone! 🗑️

---

**3) Key Differences**

**localStorage:**

- Data persists even after closing browser
- Used for long-term data (preferences, saved cart)

**sessionStorage:**

- Data removed once tab/browser is closed
- Used for temporary data (session login, one-time form state)

**4) Practical Example**

**Example: Online Shopping Cart**

- localStorage → Save user cart so even if they come tomorrow, items are still there.
- sessionStorage → Store temporary coupon code, which should vanish after session ends.

**5) Interview Q&A**

**Q1) What is the difference between localStorage and sessionStorage?**
localStorage persists data permanently (until cleared), sessionStorage stores data only for the session/tab.

**Q2) Does data in localStorage expire?**
No. It stays until you manually clear it (or user clears browser storage).

**Q3) When would you use sessionStorage?**
When you need temporary data (e.g., storing form data while filling, or session-based login).

**Q4) How much data can be stored in localStorage/sessionStorage?**
Around 5–10 MB (depends on browser).

**Q5) Are localStorage/sessionStorage secure?**

Safer than cookies (not auto-sent to server), BUT data is visible to JavaScript,

so avoid storing sensitive info like passwords.

**✅ Easy way to remember**

**localStorage** = "long-term storage" (persists forever)
**sessionStorage** = "short-term storage" (tab/session only)
