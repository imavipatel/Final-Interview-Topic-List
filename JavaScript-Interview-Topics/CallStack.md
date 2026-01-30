# Call Stack

**Call Stack :** The call stack is a fundamental concept in javascript(interpreter) that
manages the execution of function calls. It is a stack data
structure that keeps track of the function call in a program,
enusring that they are executed in correct order.

**How the call stack works:**

1. **Function Invocation:** When a function is called, it is added to the
   top of the call stack.

2. **Execution**: The javascript engine executes the function at the top
   of the stack.

3. **Function Completion:** When the function completes, it is removed from
   the top of the stack.

4. **Return to caller:** Control returns to the function that called the
   completed function, which is now at the top of the stack

Example:

Consider the following code:

```js
function firstFunction() {
  console.log("First Function");
  secondFunction();
  console.log("First Function End");
}

function secondFunction() {
  console.log("Second Function");
  thirdFunction();
  console.log("Second Function End");
}

function thirdFunction() {
  console.log("Third Function");
}

firstFunction();
```

**Call Stack Execution:**

1. **Initial Call:** firstFunction() is called and added to the call stack.
   Call Stack: [firstFunction]

2. Inside firstFunction:
   Logs "First Function".
   Calls secondFunction(), which is added to the call stack.
   Call Stack: [firstFunction, secondFunction]

3. Inside secondFunction:
   Logs "Second Function".
   Calls thirdFunction(), which is added to the call stack.
   Call Stack: [firstFunction, secondFunction, thirdFunction]

4. Inside thirdFunction:
   Logs "Third Function".
   thirdFunction completes and is removed from the call stack.
   Call Stack: [firstFunction, secondFunction]

5. Back to secondFunction:
   Logs "Second Function End".
   secondFunction completes and is removed from the call stack.
   Call Stack: [firstFunction]

6. Back to firstFunction:
   Logs "First Function End".
   firstFunction completes and is removed from the call stack.
   Call Stack: []

Output:

First Function
Second Function
Third Function
Second Function End
First Function End

**Key Points:**

1. **LIFO(Last In, First Out):** The call stack operates on a LIFO principle, meaning the last function added is
   the first to be executed and removed.

**2. Stack Overflow:** If the call stack grows too large, typically due to excessive recursion, it can result in
a "Stack Overflow" error.

**3. Single Threaded**: JavaScript is single threaded, meaning it has one call stack
and executes one piece of code at a time.

The call stack is crucial for understanding how JavaScript manages function
execution and handles synchronous code.

# Summary

- The call stack manages the execution contexts.
- It maintains the order of execution of execution contexts.
- Each new execution context is pushed onto the stack.
- The topmost execution context is the one currently being executed.
- The bottom execution context is the Global Execution Context and others are Function EC.
- When a function finishes, its execution context is popped from the stack.
- Eventually GEC is also popped from the stack, Execution of program completed.
