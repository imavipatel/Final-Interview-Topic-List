# Lexical vs Dynamic Scope

👉 Scope = "where a variable can be accessed in code".
👉 There are 2 possible models of scope in programming languages:

1.  Lexical Scope (used by JS, C, Java, etc.)
2.  Dynamic Scope (used by some older languages like Bash, Lisp)

⚡ JavaScript uses **Lexical Scope**, meaning that scope is determined
by the _physical position of code_ (where it is written).
It does NOT depend on who called the function.

# Lexical Scope Example

Here `lexical` variable is defined outside printLexical.
So printLexical() will always use the variable from where
it was **written**, not from who calls it.

```js
let lexical = "outer";

function printLexical() {
  console.log(lexical); // Looks where function is written
}

function callFn(fn) {
  let lexical = "inner"; // This doesn't matter!
  fn(); // call printLexical
}

callFn(printLexical); // "outer" (NOT "inner")

// ---------------------------------------------------------------
// Why "outer"?
// Because `printLexical` was DEFINED in the outer scope, so it
// closes over (remembers) that `lexical` variable, regardless of
// who calls it later.
```

# Dynamic Scope Example (NOT in JS, but for understanding)

- If JS were dynamically scoped, the caller’s environment would matter.
- That means printLexical would use the `lexical` from callFn.

```js
// Pseudo-code (NOT JavaScript):

let lexical = "outer";

function printLexical() {
 print lexical; // would depend on who calls me
 }

function callFn(fn) {
 let lexical = "inner";
 fn(); // would print "inner" in dynamic scoping
}

callFn(printLexical); // "inner" in dynamic languages

// 👉 This is how Bash or Lisp behave.
```

# Practical Example in JavaScript

- Inner functions always use the variable where they are defined (lexical), not from where they are called.

```js
function outer() {
  let message = "Hello from outer";

  function inner() {
    console.log(message); // closes over "message"
  }

  return inner;
}

let fn = outer();
fn(); // "Hello from outer"

// Even though outer() has finished running,
// inner() still remembers the lexical scope → closure.
```

# 🎯 Easy Theory Summary

- Lexical Scope: Based on code position (where function is written).
- Dynamic Scope: Based on call stack (who calls the function).
- JS = Lexical scope language.
- Because of lexical scope, closures are possible in JS.

# ❓ Common Interview Q&A

**Q1) What is Lexical Scope in JavaScript?**
👉 Lexical scope means a function remembers the environment
(variables) from where it was defined, not from where it is called.

**Q2) What is the difference between Lexical and Dynamic Scope?**
👉 Lexical = determined at write-time (code position).
👉 Dynamic = determined at run-time (call position).

**Q3) How do closures relate to lexical scope?**
👉 Closures exist because functions in JS remember their lexical scope
even after the outer function has finished executing.
