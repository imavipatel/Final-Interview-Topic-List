# 📘 JavaScript Notes: Strings

# 1) STRING BASICS

- Strings in JavaScript are sequences of characters.
- Strings are enclosed in quotes: single (' '), double (" "), or backticks (` `).
- Strings are IMMUTABLE → once created, they cannot be changed (operations create new strings).

```js
let str1 = "Hello";
let str2 = "World";
let str3 = `Template String`; // backticks

// Access characters
console.log(str1[0]); // "H"
console.log(str1.charAt(1)); // "e"

// Length
console.log(str1.length); // 5

// ---------------------------------------------------------------
// 2) COMMON STRING METHODS
// ---------------------------------------------------------------

let text = "JavaScript is Awesome!";

// charAt(index) → returns character at given index
console.log(text.charAt(0)); // "J"
console.log(text.charAt(4)); // "S"

// slice(start, end) → extracts substring (end not included)
console.log(text.slice(0, 10)); // "JavaScript"
console.log(text.slice(-8)); // "Awesome!"

// substring(start, end) → similar to slice but no negative indexes
console.log(text.substring(0, 10)); // "JavaScript"

// replace(search, new) → replaces first occurrence
console.log(text.replace("Awesome", "Powerful")); // "JavaScript is Powerful!"

// replaceAll → replaces all (ES2021)
console.log("aa bb aa".replaceAll("aa", "cc")); // "cc bb cc"

// match(regex) → returns array of matches
console.log(text.match(/is/g)); // ["is"]

// includes(substring) → returns true/false
console.log(text.includes("Script")); // true

// startsWith(substring) / endsWith(substring)
console.log(text.startsWith("Java")); // true
console.log(text.endsWith("!")); // true

// repeat(n) → repeats string n times
console.log("Ha".repeat(3)); // "HaHaHa"

// padStart(targetLength, padString)
console.log("5".padStart(3, "0")); // "005"

// padEnd(targetLength, padString)
console.log("5".padEnd(3, "0")); // "500"
```

# 3) TEMPLATE LITERALS

Introduced in ES6 (backticks ` `):

- Allow string interpolation with ${variable}
- Support multi-line strings easily

```js
let name = "Alice";
let age = 25;

let intro = `Hello, my name is ${name} and I am ${age} years old.`;
console.log(intro);

// Multi-line string
let poem = `Roses are red,
Violets are blue,
I love coding,
And so do you!`;
console.log(poem);
```

# 4) TAGGED TEMPLATES

- Advanced feature of template literals.
- A function processes template literal parts.

```js
function tagExample(strings, ...values) {
  console.log(strings); // literal string parts
  console.log(values); // interpolated values
  return `${values[0]} is ${values[1]} years old`;
}

let result = tagExample`Name: ${name}, Age: ${age}`;
console.log(result); // "Alice is 25 years old"
```

# 5) STRING IMMUTABILITY

- Strings cannot be changed in place.
- Modifications always return a new string.

```js
let s = "hello";
s[0] = "H"; // ❌ does nothing
console.log(s); // "hello"

s = "H" + s.slice(1); // ✅ correct way
console.log(s); // "Hello"
```

# 6) Q&A SECTION

**Q1: Difference between slice() and substring()?**
slice() → supports negative indexes
substring() → swaps arguments if start > end, no negative indexes

```js
console.log("abcdef".slice(-3)); // "def"
console.log("abcdef".substring(2, 4)); // "cd"
```

**Q2: Can strings be modified directly?**
No → they are immutable. Always returns a new string.

**Q3: Difference between includes() and match()?**
includes() → boolean (true/false)
match() → returns array of matches or null

```js
console.log("hello world".includes("world")); // true
console.log("hello world".match(/o/g)); // [ 'o', 'o' ]
```

**Q4: What are Template Literals useful for?**

- Easy interpolation of variables
- Multi-line strings
- Tagged templates for custom formatting

**Q5: What is a tagged template literal?**

- A function (tag) that processes a template string and values
- Useful for sanitizing inputs, building DSLs (like GraphQL, SQL queries)

```js
// Example: Sanitizing inputs
function safeHTML(strings, ...values) {
  return strings.reduce((result, str, i) => {
    let val = values[i - 1];
    if (typeof val === "string") {
      val = val.replace(/</g, "&lt;").replace(/>/g, "&gt;");
    }
    return result + val + str;
  });
}
let unsafe = "<script>alert('hack!')</script>";
console.log(safeHTML`<p>${unsafe}</p>`);
("<p>&lt;script&gt;alert('hack!')&lt;/script&gt;</p>");
```

**Q6: What’s the difference between padStart and padEnd?**
padStart → adds padding to the left
padEnd → adds padding to the right

```js
console.log("7".padStart(4, "0")); // "0007"
console.log("7".padEnd(4, "0")); // "7000"
```

**Q7: Why are strings immutable in JS?**

- Strings are primitive values
- Stored in fixed memory
- Makes them safe and fast (sharing between variables without accidental change)
