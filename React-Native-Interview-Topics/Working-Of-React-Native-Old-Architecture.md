# How react native works.

In React native we write our ui in react components. React Native renders them using actual native widgets on android and ios using the bridge.

# How React Native Works (The Bridge & Native Components)

In Old architecuture React Native uses Bridge as communication layer that connects the JavaScript/React Code code to native modules.

# What is the React Native Bridge

React native Old Architecture uses bridge to communicate between JavaScript and Native modules.
Because javascript and native both lives in different world.(Runtime environment) They can't call each other directly so the bridge helps them to communicate

React Native Parses the bunch of command from Javascript worlds in to a JSON array searilize it as a string and then transfer it to the native world.
By passing serialized JSON string through bridge bia the bridge.

Why JSON string?

- All messages between JS ↔ Native are converted into **JSON strings**.
- Language-neutral communication (works on iOS/Android)
- Safe transport across threads

Messages sent over the bridge operate asynchronously, meaning there’s no guarantee of immediate response.

**The Bridge in Action:**

Imagine the JS thread needs to interact with a Camera module.

1. JS thread constructs a message, serializes it as JSON before sending it across the bridge for the Native Thread.
2. The bridge optimizes and forwards the message to the native thread, where it’s decoded and processed
3. Then the native thread executes the required native code .

**MessageQueue.js** It is **Bridge** a proxy at the JS layer and all Javascript to Native and Native to JavaScript calls will be forwarded MessageQueue.js through it. There is no pointer transfer between JS and Native, all parameters are passed in strings. All instances will be numbered on both JS and Native sides, and then a mapping will be done, and then the number/string number will be used as a search basis to locate cross-border objects.

**MessageQueue.js,** This is where all the communication between the two is being handled. Messages look a bit like **JSON-RPC**, passing method calls from JavaScript to native, and callbacks from native back to JavaScript. This is the sole connection between the JavaScriptcontext and the native context. Everything is being passed through MessageQueue, every network request, network response, layout measurement, render request, user interaction, animation sequence instructions, invocation of native modules, I/O operation, you get the idea… Making sure MessageQueue isn’t congested is critical to ensure the smooth operation of our app.

React Native Old architecture has 3 main components or thread on which react native works

**JavaScript Thread** - This thread is responsible for executing and compiling all the javascript related code with the help of javascript engine (JavaScript Core, Hermes etc.)

Why important?

- All React updates (React Reconcilar) and app logic live here.
- If blocked, the app feels frozen (gestures, taps won’t respond).
- Produces a tree of React elements -> creates "shadow nodes" representation.

How it works:

1. You call setState or update props.
2. React reconciler calculates differences.
3. Update instructions are created.
4. Instructions are sent through the Bridge to Native.

Things that block JS thread:

- Long loops, heavy JSON parsing, big array processing, console.log spam.

Extra Theory:

- The JS thread has its own queue of tasks.
- Each task runs fully before the next one starts (event loop model).
- React batches multiple state updates together to reduce work.
- If a task takes too long, user input like scrolling will feel delayed.

**Native Thread/UI Thread/Main Thread** = The Native thread/ UI thread also know as main thread. It Is responsible for rendering all UI elements on the device and executing all the native code.

It handles user interactions, UI rendering, and gesture events, ensuring a seamless and responsive user experience. This thread runs natively on the device, just like in any other native application.

**The Shadow Thread/Background Thread/Layout Thread:** This is the backgroud thread where the actual layout is calculated With the help yoga layout engine.

This is used to caclulate the position of UI elements for the users screen.

# What are the performance issues and limitations?

**1. Serialization and deserialization issue** - All data had to be serialized into JSON on one side and deserialized on the other. This overhead quickly congested the bridge particularly with large data. and it is time consuming.

**2. Asynchrony and latency** => Operating in batched mode, the bridge sends messages at set intervals or when the queue reaches capacity. This introduces delays between sending and receiving messages, affecting responsiveness, especially for interactive features like animations and gestures.

**3. It is asynchronous** - All communication was one-way and asynchronous. Messages sent over the bridge operate asynchronously, meaning there’s no guarantee of immediate response. It has to wait for the response from other side.

**4. It is single-threaded** - JavaScript side work on a single thread; therefore, the computation in that world had to be performed on that single thread.

**5. JSC bundled with Android APK** - ince the “JavaScriptCore” engine is only present on iOS devices. The JSC engine needs to be part of the Android APK bundle.

**6. Native modules must load on start** - since both sides do not know each other and their object types. It creates a new type of problem that JavaScript cannot start modules when needed, applications have to load all native modules at the start of the application.

**Blog to read**

https://medium.com/@engin.bolat/is-fabric-really-faster-old-vs-new-react-native-architecture-a-real-world-benchmark-f2f639669ed4

https://anil-gudigar.medium.com/react-native-app-performance-is-a-myth-dfe7b141b812

https://rohitmondal929.medium.com/jsi-c-react-native-old-architecture-a-practical-hands-on-guide-8d8c3617a4bb

In more details

https://medium.com/@harshitkishor2/react-native-new-architecture-77335db7034f

https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e
