# Higher-Order Functions (HOFs)

# What is higher order function?

- A **higher-order function** is a function that Takes another function as an argument (callback) and Returns another function.

- They are possible because functions are first-class citizens.
- Very common in JS → map, filter, reduce are HOFs.

# ✅ Example 1: Function taking another function (callback)

```js
function greetUser(name, formatter) {
  return "Hello " + formatter(name);
}

function upperCase(str) {
  return str.toUpperCase();
}

console.log(greetUser("avi", upperCase)); // "Hello AVI"
```

# ✅ Example 2: Function returning another function

```js
function multiplier(factor) {
  return function (n) {
    return n * factor;
  };
}

let double = multiplier(2);
let triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// ✅ Example 3: Array methods (map, filter, reduce)

let numbers = [1, 2, 3, 4, 5];

let squares = numbers.map((n) => n * n); // HOF: map takes a function
console.log(squares); // [1, 4, 9, 16, 25]

let evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4]

let sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15
```

# Higher Order Function by Akshay Saini

### Q: What is Higher Order Function?

**Ans**: Higher-order functions are regular functions that take one or more functions as arguments and/or return functions as a value from it. Eg:

```js
function x() {
  console.log("Hi");
}
function y(x) {
  x();
}
y(x); // Hi
// y is a higher order function
// x is a callback function
```

Let's try to understand how we should approach solution in interview.
I have an array of radius and I have to calculate area using these radius and store in an array.

First Approach:

```js
const radius = [1, 2, 3, 4];
const calculateArea = function (radius) {
  const output = [];
  for (let i = 0; i < radius.length; i++) {
    output.push(Math.PI * radius[i] * radius[i]);
  }
  return output;
};
console.log(calculateArea(radius));
```

The above solution works perfectly fine but what if we have now requirement to calculate array of circumference. Code now be like

```js
const radius = [1, 2, 3, 4];
const calculateCircumference = function (radius) {
  const output = [];
  for (let i = 0; i < radius.length; i++) {
    output.push(2 * Math.PI * radius[i]);
  }
  return output;
};
console.log(calculateCircumference(radius));
```

But over here we are violating some principle like DRY Principle, now lets observe the better approach.

```js
const radiusArr = [1, 2, 3, 4];

// logic to calculate area
const area = function (radius) {
    return Math.PI * radius * radius;
}

// logic to calculate circumference
const circumference = function (radius) {
    return 2 * Math.PI * radius;
}

const calculate = function(radiusArr, operation) {
    const output = [];
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(operation(radiusArr[i]));
    }
    return output;
}
console.log(calculate(radiusArr, area));
console.log(calculate(radiusArr, circumference));
// Over here calculate is HOF
// Over here we have extracted logic into separate functions. This is the beauty of functional programming.

Polyfill of map
// Over here calculate is nothing but polyfill of map function
// console.log(radiusArr.map(area)) == console.log(calculate(radiusArr, area));

***************************************************
Lets convert above calculate function as map function and try to use. So,

Array.prototype.calculate = function(operation) {
    const output = [];
    for (let i = 0; i < this.length; i++) {
        output.push(operation(this[i]));
    }
    return output;
}
console.log(radiusArr.calculate(area))
```

# 🎯 Interview Q&A

**Q1: What are First-Class Functions?**
Functions treated like values (can be assigned, passed, returned).

**Q2: What is a Higher-Order Function?**
A function that takes another function as argument OR returns a function.

**Q3: Give real-world examples of HOFs in JS.**
map, filter, reduce, setTimeout, addEventListener.

**Q4: Why are HOFs powerful?**
They enable abstraction, reusability, modularity, and functional programming style.

**Q5: Can every first-class function be higher-order?**
No. First-class is the capability, HOF is a use of that capability.
