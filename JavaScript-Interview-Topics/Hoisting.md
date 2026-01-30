# Hoisting==>

Hoisting in javascript is a behavior in which a function or variable can we
used before declaration.
var is hoisted but let and const are not hoisted.
variable declared with let and const are hoisted, but they are not initialized.
Accessing them before the declaration results in a ReferenceError.

```js
//Example
//varaible hoisting with var

console.log(hoistedVar);
var hoistedVar = "This variable is hoisted";
console.log(hoistedVar);

//function hoisting
hoistedFunction();

function hoistedFunction() {
  console.log("I am hoisted function");
}

//Variable hoisting with let and const

try {
  console.log(hoistedLet);
} catch (error) {
  console.log(error);
}

let hoistedLet = "This variable is hoisted with let";

try {
  console.log(hoistedConst);
} catch (error) {
  console.log(error);
}

const hoistedConst = "This variable is hoisted with const";
```

# By Akshay Saini

Hoisting is a concept in JavaScript that allows variables and function declarations to be accessed before they are actually defined in the code. During the memory creation phase of the execution context, variables are initialized to undefined, while function declarations are stored in memory as they are.

(Same Knowledge but in points 🙃)

- Variables are initialized as undefined and function declarations are stored as they are during the memory allocation phase.
- Hoisting enables us to use variables and call functions before they are actually declared in the code.
- Using a variable or calling a function before its declaration will not result in an error, but the variable will have the value undefined until it is assigned a value.
- If a variable is not declared at all, it is considered "not defined" and will result in an error when accessed.
- Hoisting works differently for function declarations, function expressions and arrow function expression. Function declarations are fully hoisted, while function expressions and arrow function expression behave like variables and are hoisted with an initial value of undefined.
- Memory Aid:
- Variable declarations are scanned and are made undefined
- Function declarations are scanned and are made available

# Examples of Hoisting:

**Example 1:**

```js
getName(); // Namaste Javascript
console.log(x); // undefined
var x = 7;
function getName() {
  console.log("Namaste Javascript");
}
```

Below is Technical Language (Use this in Interviews)

- Despite calling the getName() function before its actual declaration, it executes successfully and prints "Namaste Javascript" because function declarations are hoisted.
- The variable x is hoisted as well but is assigned the value undefined until it is later assigned the value 7.

# Example 2:

```js
getName(); // Namaste JavaScript
console.log(x); // Uncaught Reference: x is not defined.
console.log(getName); // f getName(){ console.log("Namaste JavaScript); }
function getName() {
  console.log("Namaste JavaScript");
}
```

Below is Technical Language (Use this in Interviews)

- In this code block, we can see that the function getName() is called before its declaration. However, it executes successfully and prints "Namaste JavaScript" because function declarations are hoisted.
- The variable x is accessed, but since it is not declared, it throws an error Uncaught Reference: x is not defined.
- The console.log(getName) statement outputs the function definition as ,br /> f getName() { console.log("Namaste JavaScript"); }

# Example 3:

```js
getName(); // Uncaught TypeError: getName is not a function
console.log(getName);
var getName = function () {
  console.log("Namaste JavaScript");
};
// The code won't execute as the first line itself throws a TypeError.
```

**Below is Technical Language (Use this in Interviews)**

- In this code block, we have a function expression assigned to the variable getName.
- When getName() is called before the variable declaration, it throws a TypeError because at that point, getName is not a function but a variable with the initial value undefined.
- The console.log(getName) statement outputs the value of the variable getName, which is undefined.
- The code execution stops after the first line due to the TypeError, so the function expression is never assigned to the variable getName.

**Note:** It's important to understand the distinction between function declarations and function expressions when dealing with hoisting. Function declarations are fully hoisted, while function expressions behave like variables and are hoisted with an initial value of undefined.
