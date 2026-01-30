# Symbol datatype

🔹 Introduced in ES6
🔹 Used to create unique identifiers
🔹 Even if symbols have the same description, they are unique

```js
let sym1 = Symbol("id");
let sym2 = Symbol("id");
console.log(sym1 === sym2); // false (always unique)

let user = {
  name: "Alice",
  [Symbol("id")]: 123,
};
console.log(user); // { name: 'Alice', [Symbol(id)]: 123 }
```
