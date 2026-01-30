## Lazy loading with Code Splitting:

As a frontend engineer we aim to deliver best user experience & one of the ways to achieve that is by optimizing application performance

- User expects fast and responsive experience & quickly abandon sites that are slow to load. if web pages takes more than 3 seconds to load 40% user will leave.

- With slower internet and devices optimizing performance is necessary.

- Code splitting and lazy loading are the stratigies to achieve great performance on the web.

## Code Splitting:

- Most react apps have their files bundled using bundler **Webpack, rollup or Browserify**
- Bundling is process of following imported files & merging them into single file -> bundle
- As App grows bundle size grows when we are using third party library
- **Code Splitting** - create multiple bundler that can we loaded dynamically at run time.
- **Code Splitting** - helps app to lazy-load the things that currenly needed by the browser which dramatically improves performace,
- Reducing amount of code needed during initial load.
- Best way to introduce code splitting into your app is through dynamic import and export **ES6** syntax.

**Code Splitting with lazy loading**

- For react apps code splitting using dynamic importd id supported out of box via **React.lazy()**

## React.lazy()

- **React.lazy()** function lets u render dynamic import as a regular component which allows lazy loading of component via splitting a big JS bundle into multiple smaller JS chunks for each component that is lazy loaded.
- When Lazy component lazily loaded signifies that code for lazy component is segmented into distinct JS chunk. Seprate from main JS bundle.
- This chunks is loaded when lazy component is required to displayed on UI.

## Two methods for code splitting

**1. Route Based**
**1. Component Based**

**Route based Code Splitting**

- Route splitting is almost always best place to start code splitting & it is also where we can achieve potential max size reduction of our main js bundle.
- works best when routes are very distinct & there is very less code duplication between routes if their is there will be code duplicacy in chunks.
- Application code is divided into seprate bundles each containing JS required for specific route.
- each chunk -> specific route
- when bundler comes across dynamic import with in app automatically create seprate chunk (JS.file) which can be loaded later. so its not bundled with the main bundle

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

# Code Splitting

Code splitting allows you to split your application into smaller bundles that can be loaded independently. This technique helps reduce the initial load time by loading only the necessary code.

**For Example**
React App with Login, Dashboard, Product Listing page,

- Traditionaly code for all these pages bundled in single js files.
- With code splitting & lazy loading
  dynamically load specific comp/pages only when needed. Significantly improves performace.

- Component provides granular control over loading specific component allowing more precise option

**Real power of code splitting comes in this case**

- Large component with significant resources
- conditional component
- Secondary and non essential feature
- Critical component (Header, Footer) should load first
