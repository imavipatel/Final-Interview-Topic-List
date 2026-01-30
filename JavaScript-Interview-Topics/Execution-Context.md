# How JavaScript Works!

# Execution Context

- Everything in JavaScript happens inside the execution context.
- The execution context is like a big box or container where the JavaScript code is executed.
- It consists of two components: the memory component (variable environment) and the code component (thread of execution).

# Memory Component (Variable Environment)

- Memory component has all the variables and functions in key value pairs.
- The memory component is also known as the variable environment.

# Code Component (Thread of Execution)

- The code component is where the JavaScript code is executed line by line.
- It is also known as the thread of execution.

# JavaScript Characteristics

- JavaScript is a synchronous single-threaded language.
- Synchronous: It executes one command at a time in a specific order.
- Single-threaded: It can only execute one command at a time.
- It proceeds to the next line only when the current line has finished executing.

# Types of Execution Context:

**1. Global Execution Context:** This is the default execution context.
It is created when the JavaScript engine starts executing the code.
There is only one global execution context per

**2. Function Execution Context:** Created whenever a function is invoked. Each function has its own execution context.

**3. Eval Execution Context:** Created when code is executed inside an eval function.

**Eval :** The eval function in JavaScript is a built-in function
that evaluates a string of JavaScript code in the context of
the current execution environment

When eval is called, it creates a new execution context,
known as the Eval Execution Context. This context is
similar to the Global and Function Execution Contexts
but is created specifically for the code being evaluated
by eval.

```js
// Example :

var x = 10;
var y = 20;

function testEval() {
  var z = 30;
  eval("var result = x + y + z;");
  console.log(result); // 60
}

testEval();
```
