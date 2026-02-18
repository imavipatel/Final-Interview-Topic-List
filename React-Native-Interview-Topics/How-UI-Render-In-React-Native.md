# What Is the Render Pipeline in React Native?

In React Native’s New Architecture, turning your React code into something that shows up on the screen is done by a sequence of steps called the render pipeline. This happens every time the UI is first rendered and whenever something changes (like state or props)

**The Three Main Phases**

The render pipeline has three major phases:

1. Render
2. Commit
3. Mount

React Native goes through these steps in order to create, update, and show UI.

# Render — Build the Shadow Representation

- Your React code (written in JavaScript with JSX) makes a React Element Tree — basic objects representing your UI.

- The renderer takes that tree and makes a React Shadow Tree in C++.

- Think of the shadow tree as a mirror of your UI but in a C++ structure that React Native can work with efficiently.

- This happens synchronously and thread-safely, usually on the JavaScript thread.

**Example:**

If `<View><Text>` Hello `</Text></View> `is your code, then `<View> and <Text>` become C++ shadow nodes in the shadow tree.

**Commit — Prepare for Layout and Promote Tree**

**Once the shadow tree is built:**

- The renderer calculates layout (size and position of every element) using a layout engine like Yoga.

- After layout results are ready, the new shadow tree is marked as the “next tree” — meaning it’s ready to be shown.

These commit phase steps often run on a background thread.

**Mount — Actually Put Views on Screen**

Here’s where real UI is made visible:

- The shadow information is turned into a Host View Tree — the actual native views on Android or iOS.

- A process called diffing figures out what changed from the old UI to the new one.

- Only the minimal set of changes (like “update this View” or “remove that view”) are applied.

- Finally, native UI components are created or updated, and React Native shows them on screen.

- Mounting always happens on the UI thread, because real views must be updated there.

**A Key Idea: Immutable Trees**

Both the React Element Tree and the React Shadow Tree are immutable — they aren’t changed in place.

When something updates, a new tree is created, then compared to the old one to find the differences.

This makes updating efficient and avoids unpredictable bugs.

**Putting It All Together – At a High Level**

Here’s how it flows in a real example:

1. JavaScript creates React elements.

2. Renderer builds shadow nodes from those elements.

3. Layout of shadow nodes is calculated.

4. New tree becomes ready (“next”).

5. Differences from old UI are computed.

6. Native views are updated to reflect changes.

All of this work ensures React Native apps update UI in a predictable and optimized way

https://reactnative.dev/architecture/render-pipeline

https://reactnative.dev/architecture/view-flattening

https://reactnative.dev/architecture/glossary#react-element-tree-and-react-element

https://reactnative.dev/architecture/render-pipeline#react-state-updates

https://github.com/reactwg/react-native-new-architecture/discussions/123
