# 📘 Array Methods: map, filter, reduce, push, pop, shift, unshift reduceRight, slice, splice, flat, forEach, some, every,

- More (find, sort, reverse, join, etc.)
- Extra (fill, copyWithin, flatMap, from, of, isArray)

# 🔹 THEORY (Simple Language)

JavaScript arrays have built-in methods that help us **loop, change, search, and combine data**.  
These methods make code shorter and easier instead of writing big loops.

# 1. map()

- Makes a **new array** by applying a function on each element.
- Does not change the original array.

```js
//  ✅ Example:

let nums = [1, 2, 3, 4];
let squared = nums.map((x) => x * x);
console.log(squared); // [1, 4, 9, 16]
console.log(nums); // [1, 2, 3, 4]
```

# 2. filter()

- Makes a **new array** with only the elements that match the condition.

```js
// ✅ Example:

let even = nums.filter((x) => x % 2 === 0);
console.log(even); // [2, 4]
```

# 3. reduce()

- Combines all elements into **one single value**.

```js
// ✅ Example:

let sum = nums.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 10
```

# 4. push()

- Adds one or more elements to the **end** of an array.
- Returns the new length of the array.

```js
// ✅ Example:

let arr1 = [1, 2, 3];
let newLength1 = arr1.push(4, 5);
console.log(arr1); // [1, 2, 3, 4, 5]
console.log(newLength1); // 5
```

# 5. pop()

- Removes the **last** element from an array.
- Returns the removed element.

```js
// ✅ Example:

let arr2 = [10, 20, 30];
let removedElement1 = arr2.pop();
console.log(arr2); // [10, 20]
console.log(removedElement1); // 30
```

# 6. shift()

- Removes the **first** element from an array.
- Returns the removed element.
- All remaining elements are shifted left.

```js
// ✅ Example:

let arr3 = [100, 200, 300];
let removedElement2 = arr3.shift();
console.log(arr3); // [200, 300]
console.log(removedElement2); // 100
```

# 7. unshift()

- Adds one or more elements to the **beginning** of an array.
- Returns the new length of the array.
- All existing elements are shifted right.

# 🔹 Example 5: Combination of methods

```js
let arr5 = [];
arr5.push("first"); // ["first"]
arr5.unshift("zero"); // ["zero", "first"]
arr5.push("second"); // ["zero", "first", "second"]
arr5.shift(); // removes "zero"
arr5.pop(); // removes "second"
console.log(arr5); // ["first"]
```

```js
// ✅ Example:

let arr4 = ["b", "c"];
let newLength2 = arr4.unshift("a", "z");
console.log(arr4); // ["a", "z", "b", "c"]
console.log(newLength2); // 4
```

# 8. reduceRight()

- Same as reduce, but works from **right → left**.

```js
// ✅ Example:

let word = ["a", "b", "c"].reduceRight((acc, curr) => acc + curr);
console.log(word); // "cba"
```

# 9. slice(start, end)

- Copies part of an array into a new one.

```js
// ✅ Example:
let arr1 = [10, 20, 30, 40, 50];
console.log(arr1.slice(1, 4)); // [20, 30, 40]
```

# 10. splice(start, deleteCount, ...items)

- Changes the original array (remove/insert/replace).

```js
// ✅ Example:
let arr2 = [1, 2, 3, 4];
let removed = arr2.splice(1, 2, 9, 8);
console.log(arr2); // [1, 9, 8, 4]
```

# 11. flat(depth)

- Flattens (unpacks) nested arrays.

```js
// ✅ Example:
let nested = [1, [2, [3, [4]]]];
console.log(nested.flat(2)); // [1, 2, 3, [4]]
```

# 12. forEach()

- Runs a function for each element.

```js
// ✅ Example:

nums.forEach((x, i) => console.log(`Index ${i} → ${x}`));
```

# 13. some()

- True if **at least one element** passes.

```js
// ✅ Example:
console.log(nums.some((x) => x % 2 === 0)); // true
```

# 14. every()

- True if **all elements** pass.

```js
//✅ Example:
console.log(nums.every((x) => x > 0)); // true
```

# 15. find()

- First element matching condition.

```js
// ✅ Example:

console.log(nums.find((x) => x % 2 === 0)); // 2
```

# 16. findIndex()

- Index of first element matching condition.

```js
// ✅ Example:

console.log(nums.findIndex((x) => x % 2 === 0)); // 1
```

# 17. sort()

- Sorts array (changes original).

```js
// ✅ Example:

let letters = ["b", "a", "d", "c"];
letters.sort();
console.log(letters); // ["a", "b", "c", "d"]
```

# 18. reverse()

- Reverses array in place.

```js
// ✅ Example:

let rev = [1, 2, 3];
rev.reverse();
console.log(rev); // [3, 2, 1]
```

# 19. join(separator)

- Makes string from array.

```js
// ✅ Example:

console.log(["Hello", "World"].join(" ")); // "Hello World"
```

# 20. concat()

- Combines arrays (returns new).

```js
//✅ Example:

console.log([1, 2].concat([3, 4])); // [1, 2, 3, 4]
```

# 21. includes()

- Checks if element exists.

```js
//✅ Example:

console.log(nums.includes(2)); // true
```

# 22. indexOf()

- First index of element.

```js
//✅ Example:

console.log(["a", "b", "a"].indexOf("a")); // 0
```

# 23. lastIndexOf()

- Last index of element.

```js
//✅ Example:

console.log(["a", "b", "a"].lastIndexOf("a")); // 2
```

# 24. fill(value, start, end)

- Replaces array elements with given value.

```js
// ✅ Example:

let arr3 = [1, 2, 3, 4];
arr3.fill(0, 1, 3);
console.log(arr3); // [1, 0, 0, 4]
```

# 25. copyWithin(target, start, end)

- Copies part of array inside itself.

```js
// ✅ Example:

let arr4 = [1, 2, 3, 4, 5];
arr4.copyWithin(0, 3, 5);
console.log(arr4); // [4, 5, 3, 4, 5]
```

# 26. flatMap()

- Maps each element, then flattens one level.

```js
// ✅ Example:

let arr5 = [1, 2, 3];
console.log(arr5.flatMap((x) => [x, x * 2])); // [1, 2, 2, 4, 3, 6]
```

# 27. Array.from()

- Makes array from iterable (like string, set).

```js
// ✅ Example:

console.log(Array.from("hello")); // ["h", "e", "l", "l", "o"]
```

# 24. Array.of()

- Makes array from arguments.

```js
// ✅ Example:

console.log(Array.of(1, 2, 3)); // [1, 2, 3]
```

# 25. Array.isArray()

- Checks if value is array

```js
// ✅ Example:

console.log(Array.isArray([1, 2])); // true
console.log(Array.isArray("hello")); // false
```

# Q&A (Interview Style)

**Q1: Difference between map & forEach?**
👉 map → returns new array.  
👉 forEach → no return.

**Q2: slice vs splice?**  
👉 slice → copy, no change.  
👉 splice → edit original.

**Q3: find vs filter?**
👉 find → first match.  
👉 filter → all matches.

**Q4: indexOf vs findIndex?**  
👉 indexOf → value only.  
👉 findIndex → condition.

**Q5: flat vs flatMap?**  
👉 flat → only flattens.  
👉 flatMap → map + flatten.

**Q6: fill vs copyWithin?**  
👉 fill → replace with one value.  
👉 copyWithin → copy part inside array.

**Q7: Array.from vs Array.of?**  
👉 from → makes array from iterable.  
👉 of → makes array from arguments.

# 📊 QUICK REVISION (Cheat Sheet)

- map → new array (transform each element)
- filter → new array (only matching items)
- reduce → single value (L→R)
- reduceRight → single value (R→L)
- slice → copy part (no change)
- splice → edit original
- flat → flatten nested arrays
- forEach → loop only
- some → true if any pass
- every → true if all pass
- find → first match
- findIndex → index of first match
- sort → sort in place
- reverse → reverse in place
- join → array → string
- concat → combine arrays
- includes → check value
- indexOf → first index
- lastIndexOf → last index
- fill → replace values
- copyWithin → copy inside array
- flatMap → map + flatten
- Array.from → from iterable
- Array.of → from arguments
- # Array.isArray → check array
