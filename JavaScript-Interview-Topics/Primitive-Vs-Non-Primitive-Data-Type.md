# Primitive vs Non-Primitive Types

**🔹 Primitive Types** → stored by VALUE (immutable)

- Number, String, Boolean, Null, Undefined, Symbol, BigInt
  **🔹 Non-Primitive (Reference**) Types → stored by REFERENCE
- Objects, Arrays, Functions
  \*/

```js
let num = 42; // Number
let str = "Hello"; // String
let bool = true; // Boolean
let nothing = null; // Null
let notAssigned; // Undefined
let unique = Symbol(); // Symbol
let big = 9007199254740991n; // BigInt

let arr = [1, 2, 3]; // Array (object type)
let obj = { a: 1 }; // Object
function greet() {
  return "Hi";
} // Function

console.log(typeof num); // number
console.log(typeof obj); // object
console.log(typeof greet); // function (special object)
```
