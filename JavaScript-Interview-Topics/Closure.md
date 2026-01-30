# Closure

- Closure means in Javascript that an inner function always has
  access to the variable of it outer function even after the outer function has returned.

1. It has access to its own scope( variable defined between its curly brackets);
2. It has access to its outer functions variables
   3, It has access to the global variables.

**Example====>**

```js
let globalVariable = "I am global variable";

function outerFunction() {
  let outerVariable = "I am outer function variable";
  function innerFunction() {
    let innerVariable = "I am inner function variable";

    console.log(outerVariable);
    console.log(innerVariable);
    console.log(globalVariable);
    console.log(lowerGlobalVariable);
  }
  return innerFunction();
}

let lowerGlobalVariable = "I am lower global variable";

outerFunction();
```

**Use of Closure==>**

**1. Data Privacy and Encapsulation**
Closures allow you to create private variables that cannot be accessed
from outside the function. This is useful for
encapsulating data and protecting it from being modified directly.

```js
// Example: ==>

function createCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter());
console.log(counter());
console.log(counter());
```

**2. Function Factories:** Closures can be used to create
functions with preset configurations or behaviors.

```js
// Example:=>

function createAdder(x) {
  return function (y) {
    return x + y;
  };
}

const add = createAdder(5);

console.log(add(2));
console.log(add(10));
```

**3. Maintaining State in Asynchronous Code :** Closures help maintain state in
asynchronous operations, such as callbacks, promises, and event handlers.

```js
// Example:==>

function fetchData(url) {
  let data = null;
  fetch(url)
    .then((response) => response.json())
    .then((json) => {
      data = json;
      console.log(data); // Accessing data within the closure
    });
}

fetchData("https://api.example.com/data");
```

**4. Partial Application and Currying: **Closures enable partial application and currying,
which are techniques for fixing a number of arguments
to a function, producing another function with fewer arguments.

```js

// Example:==>

function multiply(a){
return function(b){
return a\*b;
}
}

const double = multiply(2)
console.log(double(5))
console.log(double(10));
```

**5. Event Handlers:** Closures are often used in event handlers to
maintain access to variables from the outer scope.
This allows the event handler to access and manipulate
variables that were defined outside of its own scope,
providing a powerful way to manage state and behavior in response to events.

```js
// Example:=>

// Example of using closures in event handlers
function setupButton() {
  let count = 0; // Variable in the outer scope

  document.getElementById("myButton").addEventListener("click", function () {
    count++; // Accessing and modifying the outer scope variable
    console.log(`Button clicked ${count} times`);
  });
}

// Assuming there is a button element with id 'myButton' in the HTML
setupButton();
```

**Disadvantage of Closure==>**
While closures are a powerful feature in JavaScript, they do come with some potential disadvantages.

**1. Memory Consumption:** Closures can lead to increased memory usage because
they maintain references to their outer scope.
This can prevent garbage collection of those variables,
leading to memory leaks if not managed properly.

```js
// Example:==>

function createClosure() {
  let largeArray = new Array(1000000).fill("some data");
  return function () {
    console.log(largeArray.length);
  };
}

const closure = createClosure();
// The largeArray will not be garbage collected as long as the closure exists
```

**2. Debugging Complexity:** Debugging closures can be more challenging because
it can be difficult to track the state of variables over time,
especially in complex applications with many nested functions.

3. Performance Overhead: Excessive use of closures can lead to
   performance overhead due to the additional memory and
   processing required to maintain the closure's scope chain.

4. Unexpected Behavior: If not used carefully, closures can lead
   to unexpected behavior, especially when dealing with loops
   and asynchronous code.

```js
// Example:==>

for (var i = 0; i < 5; i++) {
  setTimeout(function () {
    console.log(i);
  }, 1000);
}

//we can use let for correct output

// Correct approach using IIFE (Immediately Invoked Function Expression)
for (var i = 0; i < 5; i++) {
  (function (i) {
    setTimeout(function () {
      console.log(i); // Will log 0, 1, 2, 3, 4
    }, 1000);
  })(i);
}
```
