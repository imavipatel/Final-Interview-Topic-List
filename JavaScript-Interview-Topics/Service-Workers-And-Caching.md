## Service Workers And Cache

**What is a Service Worker?**

- A Service Worker is a special script that runs in the background
  (separate from the main browser thread).
- It can intercept network requests, cache responses, and make apps work offline.

**Rules:**

- Runs only on HTTPS (except localhost)
- It’s event-driven (install, activate, fetch events)

**Key Features:**

**Background Sync:** Once the user's device has connectivity again, perform tasks in the background such as sending form data.

**Offline Capabilities:** Cache assets and API responses to provide offline access to your app.

**Push Notifications** - Send updates to users when they may be more likely to engage with your app.

## Benefits of Service Workers

**Improved Performance:** Faster load times by caching static resources.

**Improved User Experience:** Users stay longer because app works also offline.
**Better Engagement:** Send push notification to re-engage your users.
**Less Server Load:** Cached assets means lesser and less repeated network requests.

## How Do Service Workers Work?

Service Workers follow a lifecycle with three main stages:

**Registration:** A Service Worker is registered from your main JavaScript file.
**Installation:** Browser downloads the Service Worker and installs it.
**Activation:** Once installed Service Worker starts managing requests for the app.
**Fetch:** Worker handles incoming requests

**Example: Registering a Service Worker**

```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker
    .register("/sw.js")
    .then(() => console.log("✅ Service Worker registered"))
    .catch((err) => console.log("❌ SW registration failed", err));
}
```

**Inside sw.js (Service Worker File)**

```js
// 4) Inside sw.js (Service Worker File)
// -----------------------------------------------------------------------------

// Install Event → Cache files initially
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("my-cache-v1").then((cache) => {
      return cache.addAll(["/", "/index.html", "/style.css", "/script.js"]);
    }),
  );
  console.log("📦 Service Worker: Installed and cached files");
});

// Activate Event → Cleanup old caches
self.addEventListener("activate", (event) => {
  event.waitUntil(
    caches
      .keys()
      .then((keys) =>
        Promise.all(
          keys
            .filter((key) => key !== "my-cache-v1")
            .map((key) => caches.delete(key)),
        ),
      ),
  );
  console.log("♻️ Service Worker: Activated and cleaned old caches");
});

// Fetch Event → Intercept requests
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request); // Cache-first, fallback to network
    }),
  );
});
```

**Cache API (Basics)**

```js
async function cacheExample() {
  const cache = await caches.open("my-cache");
  await cache.put("/hello.txt", new Response("Hello World!")); // Save
  const response = await cache.match("/hello.txt"); // Read
  console.log(await response.text()); // Output: Hello World!
}
cacheExample();
```

## Why Service Workers is needed?

Imagine this: You’re trying to book tickets online, but your internet connection suddenly drops. Frustrating, right? Now, imagine the same thing happening again, but the app still works and you can see cached content; it simply syncs your booking as soon as you come online again. Sounds like magic? Well, it’s not a far off reality–it’s Service Workers, one of the most powerful yet underutilized features when it comes to modern web development.

**Interview Q&A**

**Q1) What is a Service Worker?**
A script that runs in the background, intercepts requests, and enables offline caching.

**Q2) Why are Service Workers HTTPS only?**
For security reasons (they can intercept network requests, so must be secure).

**Q3) What are the main events in a Service Worker lifecycle?**
install → activate → fetch.

**Q4) How is Service Worker different from LocalStorage?**

- LocalStorage stores key-value data (synchronous).
- Service Worker manages caching of full requests/responses (async).

**Q5) Can a Service Worker update automatically?**
Yes, browser checks for updates every time the page loads.

**Easy way to remember**

**Service Worker = "Middleman between Browser & Server"**

- Install → Cache stuff initially
- Activate → Remove old junk
- Fetch → Serve cached data (or network if missing)
  Makes websites work offline like an "App".

**🍽️ Service Worker Real-World Analogy (Restaurant Waiter)**

**Imagine you go to a restaurant:**
**1) Register** → You hire a waiter (register service worker).
**2) Install**→ The waiter memorizes the menu and keeps a notepad (cache files).
**3) Activate** → Old waiters leave, only the new waiter serves you (clear old caches).
**4) Fetch** → Whenever you order food (make a network request):

- If the waiter already wrote it down in the notepad (cached response),
  he serves it instantly 🍲 (offline support).
- If not, he goes to the kitchen (fetch from server).

**✅ Benefit:**

- Faster service (cached responses).
- Even if the kitchen (server) is closed, waiter can still serve you
  something from his notes (offline).

https://dev.to/mukhilpadmanabhan/service-workers-revolutionizing-modern-web-apps-24pk
https://dev.to/jonchen/service-worker-caching-and-http-caching-p82
Important read
https://www.mbloging.com/post/js-web-service-worker-and-scheduler-api#google_vignette
