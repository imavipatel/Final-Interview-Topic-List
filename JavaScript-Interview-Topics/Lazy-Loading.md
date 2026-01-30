# Lazy Loading

Lazy loading is a technique where components, images, or other resources are loaded only when needed instead of at the initial app startup. This helps in reducing the initial bundle size and optimizing performance.

## Why is Lazy Loading Important in Web App and React Native App?

Lazy loading is beneficial in several scenarios:

**1. Faster Initial Load Time** – The app starts quickly since it doesn’t load unnecessary components immediately.
**2. Reduced Memory Usage** – Only required resources are loaded into memory, reducing RAM consumption.
**3. Optimized Performance** – Prevents unnecessary rendering of heavy components that are not in use currently, improving UI responsiveness.
**4. Lower Data Consumption** – Reduces the amount of data downloaded at startup, beneficial for users on limited data plans.

## When Should You Use Lazy Loading?

**1.** Your app has many screens or complex UI components.

**2.** You’re dealing with large images, videos, or heavy dependencies.

**3.** The initial load time is slow, affecting user experience.

**4.** You want to reduce memory usage on low-end devices.

## Common use-cases:

**1. Images on a long webpage** → load only when they are visible in viewport
**2. Modules or Libraries in a web or mobile app** → import only when needed
**3. Components in Web or Mobile App**-> like React, Vue, React Native → dynamic imports

## 1. Lazy Loading in JavaScript

**Example 1: Lazy loading images using IntersectionObserver**

```js
const images = document.querySelectorAll("img[data-src]");

const observer = new IntersectionObserver(
  (entries, observer) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        const img = entry.target;
        img.src = img.dataset.src; // load actual image
        observer.unobserve(img); // stop observing after loading
      }
    });
  },
  { threshold: 0.1 },
);

images.forEach((img) => observer.observe(img));

HTML example:
<img data-src="large-image.jpg" src="placeholder.jpg" alt="Lazy Image">
```

**Example 2: Lazy loading JS modules dynamically**

```js
document.getElementById("loadBtn").addEventListener("click", async () => {
  const module = await import("./mathModule.js");
  console.log(module.add(5, 10)); // only loaded when button clicked
});
```

**IntersectionObserver**
It monitors when an element enters the viewport and triggers the loading only then.

## How Lazy Loading works in React & React Native

**1. Use React.lazy to Lazy Load Components**

React uses **React.lazy()** to dynamically import components. This function allows you to load a component only when it is needed.

**2. Use Suspense to Wrap Lazy Loaded Components**

However, React does not automatically handle the loading state; that's where **React.Suspense** comes in. Suspense lets you specify a fallback UI that will be shown while the component is loading.

## 2. Lazy Loading in React

**Step 1: Install Required Dependencies**
Ensure you have React Router v6 installed:

```js
npm install react-router-dom@6
```

**Step 2: Define Your Lazy Loaded Components**

```js
import React, { Suspense } from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";

// Lazy load components using React.lazy
const Home = React.lazy(() => import("./Home"));
const About = React.lazy(() => import("./About"));
const Contact = React.lazy(() => import("./Contact"));

// Fallback UI component to show while lazy-loaded component is loading
const FallbackLoader = () => <div>Loading...</div>;

const App = () => {
  return (
    <Router>
      <Suspense fallback={<FallbackLoader />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/contact" element={<Contact />} />
        </Routes>
      </Suspense>
    </Router>
  );
};

export default App;
```

## Explanation:

**1. React.lazy(() => import('./Home')):**

**React.lazy** dynamically imports the component only when it's needed, reducing the initial bundle size of your application.
This is done for Home, About, and Contact components in the example.

**2.Suspense:**

The **Suspense component** is wrapped around the entire **Routes** block to provide a fallback UI **FallbackLoader** when the lazy-loaded components are being fetched.

This ensures that a loading spinner or message appears while the component is being loaded.

**3.fallback Prop in Suspense:**

The **fallback** prop specifies the UI that should be displayed while the lazy-loaded component is being fetched (in this case, it is a simple loading message).

**Lazy Loading with Nested Routes**

Lazy loading can also be applied to nested routes, improving the load time for pages with nested components.

```js
import React, { Suspense } from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";

const Dashboard = React.lazy(() => import("./Dashboard"));
const DashboardOverview = React.lazy(() => import("./DashboardOverview"));
const DashboardSettings = React.lazy(() => import("./DashboardSettings"));

const FallbackLoader = () => <div>Loading...</div>;

const App = () => {
  return (
    <Router>
      <Suspense fallback={<FallbackLoader />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/dashboard" element={<Dashboard />}>
            <Route path="overview" element={<DashboardOverview />} />
            <Route path="settings" element={<DashboardSettings />} />
          </Route>
        </Routes>
      </Suspense>
    </Router>
  );
};

export default App;
```

## Best Practices for Lazy Loading in React Router v6

**1. Chunking and Code Splitting:** React Router allows you to split your app into chunks, with each route being a separate bundle that can be loaded on demand. This reduces the initial load time and speeds up the app.

**2. Use Fallbacks Wisely:** Make sure the fallback UI provides a good user experience. A simple loading spinner or text is usually enough, but you can customize it further.

**3. Preload Critical Routes:** For critical pages that users will likely navigate to first, consider preloading these routes in the background so that they are ready when the user navigates to them.

**4. Limit Lazy Loading:** Use lazy loading for non-critical components. Do not overuse lazy loading for small components that won’t significantly affect the initial load time.

**5. Always declare them at the top level of your module:**
My lazy component’s state gets reset unexpectedly
Do not declare lazy components inside other components:

## 3. Lazy Loading in React Native

**Example: Normal Component Loading**
In this example, all components are loaded at once when the app starts, which increases the initial load time.

```jsx
import React from "react";
import HomeScreen from "./screens/HomeScreen";
import ProfileScreen from "./screens/ProfileScreen";

const App = () => {
  return (
    <>
      <HomeScreen />
      <ProfileScreen />
    </>
  );
};

export default App;
```

**Example: Lazy Loading Components**

Here, React.lazy() is used to load components only when needed, improving performance by reducing the initial load time.

```js
import React, { Suspense, lazy } from "react";
import { ActivityIndicator } from "react-native";

const HomeScreen = lazy(() => import("./screens/HomeScreen"));
const ProfileScreen = lazy(() => import("./screens/ProfileScreen"));

const App = () => {
  return (
    <Suspense fallback={<ActivityIndicator size="large" color="#0000ff" />}>
      <HomeScreen />
      <ProfileScreen />
    </Suspense>
  );
};

export default App;
```

**Lazy Loading in Navigation (React Navigation)**
Lazy loading can also be applied to navigation. Screens are loaded only when accessed, reducing initial bundle size and improving performance.

**Example: Lazy Loading Screens with Stack Navigator**

```js
import React, { Suspense, lazy } from "react";
import { createStackNavigator } from "@react-navigation/stack";
import { NavigationContainer } from "@react-navigation/native";
import { ActivityIndicator } from "react-native";

const HomeScreen = lazy(() => import("./screens/HomeScreen"));
const ProfileScreen = lazy(() => import("./screens/ProfileScreen"));

const Stack = createStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen
          name="Home"
          component={() => (
            <Suspense
              fallback={<ActivityIndicator size="large" color="#0000ff" />}
            >
              <HomeScreen />
            </Suspense>
          )}
        />
        <Stack.Screen
          name="Profile"
          component={() => (
            <Suspense
              fallback={<ActivityIndicator size="large" color="#0000ff" />}
            >
              <ProfileScreen />
            </Suspense>
          )}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
};

export default App;
```

**Lazy Loading Images**
In React Native, lazy loading images can save memory and bandwidth, especially for apps with numerous or large images.

**Example: Lazy Loading Images Using react-native-fast-image**

react-native-fast-image offers optimized image loading, with lazy loading capabilities built-in.

```js
npm install react-native-fast-image

import React from 'react';
import { ScrollView, Text } from 'react-native';
import FastImage from 'react-native-fast-image';

const LazyLoadingImages = () => {
  return (
    <ScrollView>
      <Text>Scroll down to load images</Text>
      <FastImage
        style={{ width: 200, height: 200 }}
        source={{ uri: 'https://example.com/my-image1.jpg' }}
        resizeMode={FastImage.resizeMode.contain}
      />
      <FastImage
        style={{ width: 200, height: 200 }}
        source={{ uri: 'https://example.com/my-image2.jpg' }}
        resizeMode={FastImage.resizeMode.contain}
      />
    </ScrollView>
  );
};

export default LazyLoadingImages;

```

## Lazy Loading Redux Reducers

With Redux, you can dynamically inject reducers only when needed, reducing the initial store size.

**Example: Lazy Loading Redux Reducers**

```js
// Helper function to inject a new reducer dynamically
export function injectReducer(key, asyncReducer) {
  if (!store.asyncReducers[key]) {
    store.asyncReducers[key] = asyncReducer;
    store.replaceReducer(createReducer(store.asyncReducers));
  }
}
```

## Lazy Loading Libraries and Data

Lazy loading third-party libraries and large datasets can further improve app performance.

**Example: Lazy Loading a Library**

```js
import React, { lazy, Suspense, useState } from "react";

const HeavyComponent = lazy(() => import("heavy-library"));

const App = () => {
  const [showComponent, setShowComponent] = useState(false);

  return (
    <View>
      <Button title="Load Component" onPress={() => setShowComponent(true)} />
      {showComponent && (
        <Suspense fallback={<Text>Loading...</Text>}>
          <HeavyComponent />
        </Suspense>
      )}
    </View>
  );
};

export default App;
```

## 1. Lazy Loading and Offline-First Apps

For apps designed to work offline, lazy loading gets tricky. Components need to be available without an internet connection, but you can still defer rendering non-essential parts.
**Unique Technique: Prefetching for Offline Use**

Instead of simply lazy loading, combine lazy loading with prefetching. Here's how:

```js
import React, { useState, useEffect, lazy, Suspense } from "react";
import { View, Text, Button } from "react-native";

const LazyComponent = lazy(() => import("./LazyComponent"));

const App = () => {
  const [isReadyOffline, setIsReadyOffline] = useState(false);

  useEffect(() => {
    // Simulate prefetching and offline preparation
    const timer = setTimeout(() => setIsReadyOffline(true), 2000);

    return () => clearTimeout(timer);
  }, []);

  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      {isReadyOffline ? (
        <Suspense fallback={<Text>Loading...</Text>}>
          <LazyComponent />
        </Suspense>
      ) : (
        <Text>Preparing offline resources...</Text>
      )}
    </View>
  );
};

export default App;
```

## 2. Conditional Lazy Loading

Most examples show static lazy loading, but what if you need it based on runtime conditions? For instance, in a multi-theme app, you might load components differently based on the selected theme.

**Example: Lazy Loading Based on Theme**

```js
import React, { lazy, Suspense, useState } from "react";
import { View, Button, Text } from "react-native";

// Lazy load components for light and dark themes
const LightThemeComponent = lazy(() => import("./LightThemeComponent"));
const DarkThemeComponent = lazy(() => import("./DarkThemeComponent"));

const App = () => {
  const [theme, setTheme] = useState("light");

  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      <Button
        title="Toggle Theme"
        onPress={() => setTheme(theme === "light" ? "dark" : "light")}
      />
      <Suspense fallback={<Text>Loading theme...</Text>}>
        {theme === "light" ? <LightThemeComponent /> : <DarkThemeComponent />}
      </Suspense>
    </View>
  );
};

export default App;
```

## 3. Lazy Loading with Animations

Lazy loading combined with animations can create visually appealing transitions that make loading less noticeable.

**Example: Lazy Loading with a Fade-In Effect**

```js
import React, { lazy, Suspense, useRef, useEffect } from "react";
import { View, Animated, Text } from "react-native";

const LazyComponent = lazy(() => import("./LazyComponent"));

const App = () => {
  const fadeAnim = useRef(new Animated.Value(0)).current; // Initial opacity 0

  useEffect(() => {
    Animated.timing(fadeAnim, {
      toValue: 1,
      duration: 1000,
      useNativeDriver: true,
    }).start();
  }, []); // Empty dependency array ensures this runs only once

  return (
    <Suspense fallback={<Text>Loading...</Text>}>
      <Animated.View style={{ flex: 1, opacity: fadeAnim }}>
        <LazyComponent />
      </Animated.View>
    </Suspense>
  );
};

export default App;
```

## Tip: Avoid module side effects

Lazy-loading components can change the behavior of your app if your component modules (or their dependencies) have side effects, such as modifying global variables or subscribing to events outside of a component. Most modules in React apps should not have any side effects.

```js
import Logger from "./utils/Logger";

//  🚩 🚩 🚩 Side effect! This must be executed before React can even begin to
// render the SplashScreen component, and can unexpectedly break code elsewhere
// in your app if you later decide to lazy-load SplashScreen.
global.logger = new Logger();

export function SplashScreen() {
  // ...
}
```

## 🔹 2. Inline Requires

- By default, React Native loads all `require/import` statements at startup.
- **Inline requires** delays loading of a module until it's actually used.
- Example:
  ❌ Without inline require → entire file loaded at startup.
  ✅ With inline require → file loaded only when function/component is called.

✅ Benefits:

- Smaller JS execution at startup.
- Faster time-to-interactive for first screen.

⚠️ Downsides:

- Small delay when module is used for the first time.
- Works best when used with RAM bundles.

**🔹 Example:**

```js
metro.config.js;
module.exports = {
  transformer: {
    getTransformOptions: async () => ({
      transform: {
        experimentalImportSupport: false,
        inlineRequires: true, // enable inline requires
      },
    }),
  },
};
```

**Example Explanation:**

- `inlineRequires: true` means RN won’t load every module on startup.
- Instead, it loads modules only when they are needed.

**Advanced: Automatically inline require calls**

If you use the React Native CLI to build your app, require calls (but not imports) will automatically be inlined for you, both in your code and inside any third-party packages (node_modules) you use.

```js
import { useCallback, useState } from "react";
import { TouchableOpacity, View, Text } from "react-native";

// This top-level require call will be evaluated lazily as part of the component below.
const VeryExpensive = require("./VeryExpensive").default;

export default function Optimize() {
  const [needsExpensive, setNeedsExpensive] = useState(false);
  const didPress = useCallback(() => {
    setNeedsExpensive(true);
  }, []);

  return (
    <View style={{ marginTop: 20 }}>
      <TouchableOpacity onPress={didPress}>
        <Text>Load</Text>
      </TouchableOpacity>
      {needsExpensive ? <VeryExpensive /> : null}
    </View>
  );
}
```

## Monitor Bundle Size:

Use tools like Webpack Bundle Analyzer to identify large components.

**Conclusion**

Lazy loading and code splitting are essential techniques for optimizing React Native apps. By loading only the necessary components, screens, libraries, or data when needed, you can significantly reduce initial load times, improve performance, and create a smoother user experience.

## Real-World Insights and Best Practices

**1- Analyze Usage Patterns:** Lazy load frequently accessed resources early in idle times (e.g., while the user is reading or interacting with another component).

**2- Combine Lazy Loading with Data Fetching:** If your component relies on API data, consider triggering both simultaneously.

**3- Monitor Performance in Low-End Devices:** Always test lazy loading impact on slower devices to ensure improvements.

## Link to Read

https://dev.to/ajmal_hasan/master-lazy-loading-concept-in-react-nativereactjs-3cfe

https://dev.to/naly_moslih/lazy-loading-in-react-native-beyond-the-basics-25lo

https://dev.to/amitkumar13/lazy-loading-in-react-native-boost-performance-and-optimize-resource-usage-1fmj

https://dev.to/joodi/lazy-loading-in-react-4gdg

https://dev.to/abhay_yt_52a8e72b213be229/optimizing-performance-with-lazy-loading-in-react-router-v6-3m5p
