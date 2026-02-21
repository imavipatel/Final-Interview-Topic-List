# What is memory leak in react native.

- A memory leak happens when your app keeps holding onto memory that's no longer needed. This causes: ⚡ Performance issues — App gets sluggish over time 📉 High memory usage — Can lead to app crashes 🔄 Battery drain — Drains device resources unnecessarily

- Memory leaks in React Native can lead to serious performance issues, including app slowdowns and crashes. A memory leak occurs when objects that are no longer needed are not properly cleaned up, causing the app’s memory usage to grow uncontrollably

Native leaks can occur on both sides:
• JS-side leaks (closures, timers, listeners, unremoved callbacks, large retained arrays)
• Native-side leaks (native modules, views not released, unmanaged native caches)

# Common Cases of memory leak in react native

**1. Unsubscribed Event Listeners**

Adding event listeners without removing them when the component unmounts can cause memory leaks.

```jsx
import { useEffect } from "react";
import { AppState } from "react-native";

const App = () => {
  useEffect(() => {
    const handleAppStateChange = (nextAppState) => {
      console.log("App state changed:", nextAppState);
    };

    AppState.addEventListener("change", handleAppStateChange);

    // ❌ Missing cleanup function leads to memory leak
  }, []);

  return null;
};
```

**Problem:**
When the component re-renders or unmounts, a new event listener is added each time without removing the previous one.

**Solution: Remove the Listener**

```jsx
useEffect(() => {
  const handleAppStateChange = (nextAppState) => {
    console.log("App state changed:", nextAppState);
  };

  AppState.addEventListener("change", handleAppStateChange);

  return () => {
    AppState.removeEventListener("change", handleAppStateChange);
  };
}, []);
```

**2. Not Clearing Timers**

Using **setTimeout, setInterval, or requestAnimationFrame** without clearing them can cause memory leaks.

**Example of Memory Leak**

```jsx
import { useEffect } from "react";

const TimerComponent = () => {
  useEffect(() => {
    const intervalId = setInterval(() => {
      console.log("Running interval...");
    }, 1000);

    // ❌ Interval keeps running even after component unmounts
  }, []);

  return null;
};
```

**Solution: Clear Timers in Cleanup**

```jsx
useEffect(() => {
  const intervalId = setInterval(() => {
    console.log("Running interval...");
  }, 1000);

  return () => {
    clearInterval(intervalId);
  };
}, []);
```

**3. Async Operations Running After Unmount**

If an API call or async function **(fetch/promise)** keeps running after a component is unmounted, it can cause memory leaks.

**Example of Memory Leak**

```jsx
import { useState, useEffect } from "react";

const LargeObjectComponent = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("https://example.com/large-data")
      .then((response) => response.json())
      .then((result) => setData(result));

    // ❌ No cleanup to handle component unmounting
  }, []);

  return null;
};
```

**Solution: Use Cleanup for Large Data**

```jsx
useEffect(() => {
  let isMounted = true;

  fetch("https://example.com/large-data")
    .then((response) => response.json())
    .then((result) => {
      if (isMounted) {
        setData(result);
      }
    });

  return () => {
    isMounted = false;
  };
}, []);
```

**4. Retaining References in Closures**

Using outdated state references in closures can cause memory leaks if the component re-renders frequently.

**Example of Memory Leak**

```jsx
import { useState, useEffect } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setCount(count + 1); // ❌ Stale closure issue
    }, 1000);

    return () => clearInterval(intervalId);
  }, []);

  return <Text>{count}</Text>;
};
```

**Solution: Use Functional Updates**

```jsx
useEffect(() => {
  const intervalId = setInterval(() => {
    setCount((prevCount) => prevCount + 1); // ✅ Correctly updates state
  }, 1000);

  return () => clearInterval(intervalId);
}, []);
```

**5. Memory Leaks in Navigation**

If you perform operations like API calls or set state inside useEffect without checking if the component is still mounted, it can lead to memory leaks.

**Example of Memory Leak**

```jsx
import { useState, useEffect } from "react";

const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`https://api.example.com/user/${userId}`)
      .then((res) => res.json())
      .then((data) => setUser(data)) // ❌ Can update state even after unmounting
      .catch((error) => console.error(error));
  }, [userId]);

  return null;
};
```

**Solution: Cancel Requests if Unmounted**

```jsx
useEffect(() => {
  let isMounted = true;

  fetch(`https://api.example.com/user/${userId}`)
    .then((res) => res.json())
    .then((data) => {
      if (isMounted) setUser(data);
    })
    .catch((error) => console.error(error));

  return () => {
    isMounted = false;
  };
}, [userId]);

// ✅ Fix: Use Abort Controller

useEffect(() => {
  const controller = new AbortController();

  fetch("https://api.example.com/data", { signal: controller.signal })
    .then((res) => res.json())
    .then((data) => console.log(data))
    .catch((err) => {
      if (err.name === "AbortError") {
        console.log("Fetch aborted");
      }
    });

  return () => controller.abort(); // ✅ Cancel request on unmount
}, []);
```

**6. Native modules and subscriptions**

Native event emitters can hold references to JS callbacks. Without cleanup, they stay alive even after the component disappears. The Medium article shows an event subscription added via NativeEventEmitter and emphasises removing it in the cleanup function:

```jsx
import { NativeEventEmitter, NativeModules, useEffect } from "react-native";

const eventEmitter = new NativeEventEmitter(NativeModules.MyNativeModule);
export function MyComponent() {
  useEffect(() => {
    const subscription = eventEmitter.addListener("MyEvent", (event) => {
      console.log(event);
    });
    return () => {
      subscription.remove();
    };
  }, []);
  return null;
}
```

**7. Custom hook: useLeakSafeEffect**

To standardize cleanup and avoid stale closures, you can create a hook that tracks whether the component is mounted and ensures asynchronous work is cancelled appropriately. The following hook wraps useEffect and passes a flag to the effect callback:

```jsx

import { useEffect, useRef } from 'react';

/**
 * useLeakSafeEffect: runs an effect that can check if the component is still mounted.
 * The callback receives an object with `isMounted` and should return a cleanup function.
 */
export function useLeakSafeEffect(
  callback: (api: { isMounted: () => boolean }) => void | (() => void),
  deps: React.DependencyList
) {
  const mountedRef = useRef(true);
  useEffect(() => {
    mountedRef.current = true;
    const cleanup = callback({ isMounted: () => mountedRef.current });
    return () => {
      mountedRef.current = false;
      if (typeof cleanup === 'function') cleanup();
    };
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, deps);
}
// Usage example
useLeakSafeEffect(({ isMounted }) => {
  const ctrl = new AbortController();
  fetch(url, { signal: ctrl.signal })
    .then((res) => res.json())
    .then((json) => {
      if (isMounted()) setData(json);
    });
  return () => {
    ctrl.abort();
  };
}, [url]);
```

By using **useLeakSafeEffect,** you ensure that asynchronous operations check isMounted before updating state and always cancel in‑flight requests on unmount.

**5. Over‑rendering and large lists**
Excessive renders cause unnecessary memory allocations. Use React.memo and memoize functions with useCallback to prevent re‑creating functions on every render. When using FlatList, supply a stable keyExtractor and set initialNumToRender to limit the number of items rendered at once. For example:

```jsx
const Item = React.memo(({ item }: { item: ItemType }) => {
  return <MyItemComponent data={item} />;
});

function MyListScreen({ data }: { data: ItemType[] }) {
  const renderItem = useCallback(({ item }) => <Item item={item} />, []);
  const keyExtractor = useCallback((item: ItemType) => item.id.toString(), []);
  return (
    <FlatList
      data={data}
      renderItem={renderItem}
      keyExtractor={keyExtractor}
      initialNumToRender={10}
    />
  );
}
```

**Best Practices to Prevent Memory Leaks**

**1. Always cleanup event listeners:** Use the return function in useEffect to remove listeners.

**2. Clear timers and intervals:** Use clearTimeout and clearInterval to avoid them running after unmounting.

**3. Use functional updates for state:** Prevent stale closures when updating state inside effects.

**4. Cancel API requests on unmount:** Track the component’s mount state before updating the state.

**5. Optimize component re-renders:** Use React.memo, useMemo, and useCallback to prevent unnecessary renders.

# How to Debug Memory Leaks in React Native

**1 Use Chrome DevTools for Memory Profiling**

1️⃣ Run your app in debug mode 2️⃣ Open Chrome DevTools (Ctrl + Shift + I) 3️⃣ Go to Performance Tab → Click "Record" 4️⃣ Perform actions in your app and watch for memory spikes

📌 Tip: If memory usage keeps increasing without dropping, you have a leak!

**2 Use React Native's Performance Monitor**

React Native comes with a built-in performance monitor to track memory usage.

**Run this in your terminal:**

Copy
adb shell dumpsys meminfo your.package.name
🔍 Look for increasing heap size over time.

**3 Use the why-did-you-render Library**

If unnecessary re-renders are causing memory issues, install this:

Copy
npm install @welldone-software/why-did-you-render
Then, add this in your App.tsx:

Copy
import React from "react";
import whyDidYouRender from "@welldone-software/why-did-you-render";

whyDidYouRender(React, {
trackAllPureComponents: true,
});

🚀 Now, you can track which components are re-rendering unnecessarily!

**4. Android Studio Profiler & Xcode Instruments**

For native leaks, you need platform‑specific tools. Umair Zafar's article recommends the Android Studio Memory Profiler and Xcode Instruments (Leaks Tool) to monitor heap memory and locate objects that were not deallocated. These tools are essential for identifying leaks in native modules or third‑party libraries.

# Blogs to read

https://blog.swmansion.com/hunting-js-memory-leaks-in-react-native-apps-bd73807d0fde

https://medium.com/@engin.bolat/performance-profiling-101-how-to-find-the-bottleneck-in-10-minutes-c22e194397ee

https://freedium-mirror.cfd/https://medium.com/@mohantaankit2002/debugging-memory-leaks-in-react-native-apps-common-causes-and-fixes-e013405317fe
