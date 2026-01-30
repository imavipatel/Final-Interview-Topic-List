# Cookies vs 🗄️ IndexedDB — JS Notes

# 🍪 Cookies vs 🗄️ IndexedDB — JS Notes

**📝 Theory**

- Cookies and IndexedDB are both browser storage mechanisms, but they are used for VERY different purposes.

**Cookies** = small data, mainly for server communication

**IndexedDB** = large structured data, client-side database

# 🍪 Cookies

**What are Cookies?**

- Accessible by client side as well as server side JS.

- Storing small piece of data (like sessionId, JWT) that need to sent back to server with every HTTP req.

- Secure (HTTPS) Authentication

- Small pieces of data stored in the browser

- Automatically sent to server with every HTTP request

- Mainly used for authentication & session management

**🔹 Key Points**

- Max size: ~4 KB per cookie

- Can have expiry time

- Accessible by server + client (unless HttpOnly)

- Slower if overused (sent with every request)

```js
// Example
document.cookie = "token=abc123; expires=Fri, 31 Dec 2026 12:00:00 UTC; path=/";

// Read cookies
console.log(document.cookie);
```

**Common Use Cases**

- Login session (JWT / session id)

- Remember user (Remember Me)

- Server-side authentication

# 🗄️ IndexedDB

**What is IndexedDB?**

IndexedDB is a **browser-based, client-side database**.
It is mainly used in **Web apps (React, PWA)** for storing large,
structured data and enabling **offline-first behavior**.

- A full-featured client-side database

- Stores large amounts of structured data

- Faster than localStorage for big data

- Supports objects, arrays, files, blobs

- Works asynchronously (non-blocking)

**Key Points**

- **Storage size:** Hundreds of MB (browser dependent)
- Not sent to server automatically
- Faster for large datasets
- Complex but powerful

```js
// Example: 1
const request = indexedDB.open("MyDatabase", 1);

request.onupgradeneeded = (event) => {
  const db = event.target.result;
  db.createObjectStore("users", { keyPath: "id" });
};

request.onsuccess = (event) => {
  const db = event.target.result;
  const tx = db.transaction("users", "readwrite");
  const store = tx.objectStore("users");

  store.add({ id: 1, name: "Avi", role: "Admin" });
};

// Example 2: Cache API response locally

const request = indexedDB.open("AppDB", 1);

request.onupgradeneeded = (event) => {
  const db = event.target.result;
  db.createObjectStore("products", { keyPath: "id" });
};

request.onsuccess = (event) => {
  const db = event.target.result;
  const tx = db.transaction("products", "readwrite");
  const store = tx.objectStore("products");

  store.put({ id: 1, name: "Laptop", price: 50000 });
};

// Fetch from IndexedDB later
function getProducts(db) {
  return new Promise((resolve) => {
    const tx = db.transaction("products", "readonly");
    const store = tx.objectStore("products");
    const request = store.getAll();

    request.onsuccess = () => resolve(request.result);
  });
}
```

**Common Use Cases**

- Offline-first apps
- Large datasets (chat history, products)
- Storing large lists (products, messages)
- Caching API responses
- Progressive Web Apps (PWA)

**Key Differences**

**Cookies**

- Max size ~4 KB

- Sent automatically with HTTP requests

- Mostly used for auth/session

- Can be accessed by server

**IndexedDB**

- Very large storage (MBs)

- NOT sent to server automatically

- Used as client-side database

- Asynchronous & fast for big data

# Practical Examples

**Example: E-commerce App**

**Cookies**→ Store auth token (session id / JWT)

**IndexedDB** → Store product list, cart items, offline orders

# Example: Chat App

**Cookies** → User login session

**IndexedDB** → Chat history for offline access

# Interview Q&A

**Q1) What is the main difference between Cookies and IndexedDB?**
**Cookies** are small and sent with every request to the server,
while **IndexedDB** is a client-side database for large structured data.

**Q2) Why not store large data in cookies?**
Cookies have size limits (~4KB) and slow down network requests because they are sent with every HTTP call.

**Q3) Is IndexedDB synchronous?**

No. IndexedDB is asynchronous to avoid blocking the main thread.

**Q4) Can IndexedDB replace cookies?**

No. Cookies are required for server-side authentication,
while IndexedDB is for client-side storage only.

**Q5) Is IndexedDB secure?**
Safer than cookies for large data, but still accessible via JavaScript,
so sensitive data should be encrypted.

**Easy Way to Remember**

Cookies = Small + Server communication

IndexedDB = Big + Client-side database
