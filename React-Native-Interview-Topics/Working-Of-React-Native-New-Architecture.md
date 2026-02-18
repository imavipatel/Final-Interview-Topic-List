# Working of React Native New Architecture

The New Architecture dismantles the Bridge bottleneck and replaces it with three tightly coupled components:

React native new architecture changes how javascript talks with native (Android/iOS) code so apps can be faster, efficient and smoother.

**New Architecture have 4 main things to understand.**

1. JSI (JavaScript Interface): The Bridge Killer

2. TurboModules: For Faster Startup Time

3. Fabric: The Concurrent Renderer

4. CodeGen: Static Type Checking and Generative native Code.

**JSI (JavaScript Interface): The Bridge Killer**

JSI stands for JavaScript Interface Written in C++.
It replaces the bridge and allows Javacript code to hold the direct refrences to native modules via C++ host objects. And now JavaScript can directly call the native methods using that refrence.

This reference object helps in synchronous communication between JavaScript and Native thread.

Whenever the React Native layer wants to update the UI screen, React Native send message synchronously to the Native side and Native layer acts accordingly.

This JSI is written in C++, that’s why it’s not limited to just Android or iOS. With the power of C++ React Native can target large number of Systems like Smart TVs, Watches etc.

**TurboModules: For Faster Startup Time**

Turbo Modules is just a Native Module system that replaced the OLD Native Module system.

In the old architecture, all the Native Modules used by JavaScript (e.g. Camera, Bluetooth, Geo Location, File Storage, etc) have to be initialized and loaded into the memory before the app is opened. This means, that even if the user doesn’t require a particular module, it still has to be initialized at start-up. And Its increase the Apps time to Interactive (TTI)

**Interview Ready** - TurboModules allows Native Modules to be lazy-loaded — they are only loaded when they are actually called for the first time. Because JavaScript will be able to hold reference to these modules, which will allow JS Code to load each module only when it is required. This will significantly reduces the initial work and improves application launch speed.

**Fabric: The Concurrent Renderer**
Fabric is the new UIManager in React Native.
It is the new rendering system responsible for creating and updating UI on iOS and Android devices.

**🔹 Why Fabric Was Introduced?**

**In the old architecture:**
React Native maintained two separate hierarchies (JS side & Native side).

Communication between them was slower.
Memory usage was higher.
Fabric improves this.

Fabric is react native new rendering system. With new rendering system user interaction such as scrolling, gestures etc can be prioritized to be executed synchronously in the main thread or native thread. While other tasks such as API requests will be executed asynchronously.

Fabric exposes its functions via JavaScript so the JS side and Native side (vice-versa) can communicate directly through ref functions. passing data between sides will be performant.

That’s not all. The new Shadow Tree will be immutable, and it will be shared between the JS and UI threads, for allowing straight interaction from both ends.

As we have seen, in the current architecture React Native has to maintain two hierarchies/DOM nodes. But since the shadow tree will now be shared among realms, it will help with reducing memory consumption as well.

The React Native renderer goes through a sequence of work to render React logic to a host (iOS, Android) platform. This sequence of work is called the render pipeline and occurs for initial renders and updates to the UI state.

# For Interview

**🔹 What is Fabric?**

Fabric is the new UIManager in React Native.
It is the new rendering system responsible for creating and updating UI on iOS and Android devices.

**Why Fabric Was Introduced?**

In the old architecture:
React Native maintained two separate hierarchies (JS side & Native side).
Communication between them was slower.
Memory usage was higher.
Fabric improves this.

**1. Concurrent Rendering (Priority-Based Updates)**
Fabric allows prioritization of tasks.
User interactions (scrolling, gestures, touch events)
→ Executed synchronously on the main/UI thread
→ Makes UI smooth and responsive

Non-urgent tasks (API calls, background work)
→ Executed asynchronously
👉 This improves performance and responsiveness.

**2. Direct JS ↔ Native Communication**

Fabric exposes native functions to JavaScript.

This means:
JS can call native directly
Native can call JS directly
Data passing is more efficient
Performance is better

**3. Immutable Shared Shadow Tree**

In Fabric:
The Shadow Tree is immutable (it cannot be modified directly).
A new tree is created on every update.
The tree is shared between JS and UI threads.

Benefits:
No need to maintain two separate hierarchies
Less memory consumption
Safer concurrent updates
Faster interaction between threads

**4. Render Pipeline Still Exists**

Fabric still follows the same render pipeline process:
Render
Commit
Mount

This happens:
On initial render
On every UI update
But now it is more optimized and concurrent.

# Interview Answers

Fabric is React Native’s new rendering system and new UIManager. It enables concurrent rendering, meaning high-priority tasks like gestures and scrolling run synchronously on the main thread, while background tasks run asynchronously. It introduces an immutable shadow tree shared between JS and UI threads, reducing memory usage and improving performance. Communication between JS and native is also more direct and efficient in Fabric.

# Testing of Fabric

Test 4: The Jank Test (JS Thread Congestion)
Scenario: A complex, continuous animation (e.g., a scrolling ticker) is running on the screen while the JS Thread is purposefully blocked for 2 seconds with a heavy computation (e.g., a large while loop or data sort).
Architectural Target: Fabric (UI Thread Decoupling).
Analysis: The Real Value Proposition. In the Legacy Architecture, the animation immediately froze (jank) because it relied on the blocked JS thread.With Fabric, the animation continued running flawlessly at 60 FPS. This is the fundamental difference: Fabric moves the rendering logic to a separate thread, meaning even if your JavaScript code is struggling, the user interface remains responsive.

**The render pipeline can be broken into three general phases:**

**Render:** React executes code logic which creates React Element Trees in JavaScript. From this React Element Trees tree, the renderer creates a React Shadow Tree in C++.

**Commit:** After a React Shadow Tree is fully created, the renderer triggers a commit. This promotes both the React Element Tree and the newly created React Shadow Tree as the “next tree” to be mounted. This also schedules calculation (with help of Yoga layout manager) of its layout information.

**Mount:** The React Shadow Tree, now with the results of layout calculation, is transformed into a Host View Tree.

**CodeGen**

A strongly typed language enforces strict rules on data types, requiring variables to have a defined type that cannot be implicitly changed

C++ is a strongly type language and JavaScript/TypeScript is not a strongly typed language. To communicate between both javascript and C++ there must be some interface. CodeGen is a tool that helps to create the interface from the JavaScript/TypeScript that generated interfaces help to communicate between both the C++ world and the JavaScript world.

The “CodeGen” is not a proper pillar, but it is a tool that can be used to avoid writing a lot of repetitive code. Using “CodeGen” is not mandatory: all the code that is generated by it can also be written manually. However, it generates scaffolding code that could save you a lot of time. The “CodeGen” is invoked automatically by React Native every time an iOS or Android app is built.

**Codegen** especially do these **2 jobs** in the new architecture of React Native

**Static type checking:**

**JavaScript is a dynamically typed** (you don’t need to define the type of a variable) language, and JSI is written in **C++, which is a statically typed** (you must define variable type) language. Consequently, there are some communication issues between the two (JS and C++).

That’s why the new architecture includes a static type checker called CodeGen. It solves the type-related communication issue between JavaScript & C++.

**Generate “Native code”:**

Codegen is responsible for generating Native Code for 2 big components of New Architecture. These are:

The Turbo Module component

“typed JavaScript code” to define the Native components or Native modules that you want to use in your app.

The Fabric component

the JavaScript code of the app defines user interface elements using React components, which are pieces of code that describe how a part of the app should look and behave. React components are translated into native code using a feature called Fabric, which is a new rendering system for React Native. Fabric also uses JSI to communicate with JavaScript and native code, without using the bridge. Fabric improves the speed and responsiveness of user interface updates, as well as integrates better with native features.

**Blog to read**

https://medium.com/@engin.bolat/is-fabric-really-faster-old-vs-new-react-native-architecture-a-real-world-benchmark-f2f639669ed4

https://anil-gudigar.medium.com/react-native-app-performance-is-a-myth-dfe7b141b812

https://rohitmondal929.medium.com/jsi-c-react-native-old-architecture-a-practical-hands-on-guide-8d8c3617a4bb

In more details

https://medium.com/@harshitkishor2/react-native-new-architecture-77335db7034f

https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e
