# 📌 PROTOTYPE, **proto**, PROTOTYPE CHAIN & INHERITANCE

## 📝 DETAILED THEORY:

**1. What is a Prototype?**

- In JavaScript, every object is linked to another object called its "prototype".
- The prototype acts like a fallback object → if JS doesn’t find a property/method on the object, it searches in its prototype.
- This mechanism is called PROTOTYPAL INHERITANCE.

**2. **proto** (Dunder Proto)**

- Every object in JS has a hidden property called `__proto__`.
- It is a reference to the prototype from which that object is inherited.
- You can access it directly (not recommended in production, but useful for learning/debugging).

**3. Prototype Property (on Functions)**

- Functions in JS automatically get a `prototype` property.
- When you use a function as a constructor (`new` keyword), the created object’s `__proto__` points to the function’s `prototype`.

**4. Prototype Chain**

- When you try to access a property on an object:
  - JS looks inside the object.
  - If not found, it goes to the object’s prototype (`__proto__`).
  - If still not found, it keeps moving up until `Object.prototype`.
  - If nothing is found even there → returns `undefined`.
- This lookup path is called the "Prototype Chain".

**5. End of Prototype Chain**

- The chain ends at `Object.prototype.__proto__` which is `null`.

**6. Inheritance**

- JS uses prototypes to implement inheritance.
- With ES6, `class` and `extends` were introduced → but internally they still use prototypes.

```js
/*
-----------------------------------------------------
🔹 PART 1: __proto__
-----------------------------------------------------
*/

const person = {
  greet: function () {
    console.log("Hello from person");
  },
};

const student = {
  name: "Avi",
};

// Link student to person
student.__proto__ = person;

student.greet(); // ✅ "Hello from person" (inherited from prototype)

console.log(student.__proto__ === person); // true

/*
-----------------------------------------------------
🔹 PART 2: Prototype Chain
-----------------------------------------------------
*/

console.log(student.__proto__ === person); // true
console.log(person.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null

/*
-----------------------------------------------------
🔹 PART 3: Function Prototypes
-----------------------------------------------------
*/

function Animal(name) {
  this.name = name;
}

// Shared method for all Animal objects
Animal.prototype.speak = function () {
  console.log(`${this.name} makes a sound`);
};

const dog = new Animal("Dog");
dog.speak(); // ✅ "Dog makes a sound" (inherited from Animal.prototype)

/*
-----------------------------------------------------
🔹 PART 4: Class Inheritance (syntactic sugar)
-----------------------------------------------------
*/

class Car {
  drive() {
    console.log("Driving a car...");
  }
}

class Tesla extends Car {
  autopilot() {
    console.log("Self-driving mode enabled 🚗🤖");
  }
}

const myCar = new Tesla();
myCar.drive(); // ✅ Inherited from Car
myCar.autopilot(); // ✅ Defined in Tesla
```

# Q&A (Interview Style)

**Q1: What is **proto** in JavaScript?**
👉 It's a reference to the prototype object from which another object inherits.

**Q2: What’s the difference between prototype and **proto**?**
👉 - prototype → property of constructor functions/classes (used for inheritance).

- **proto** → property of objects, points to their prototype.

**Q3: How does the prototype chain work?**

👉 If a property is not found on the object, JS looks into its **proto**. This continues until `null`.

**Q4: What is the end of the prototype chain?**
👉 null (after `Object.prototype.__proto__`).

**Q5: How does inheritance work in JavaScript?**
👉 Objects/classes inherit properties and methods via prototypes or the `extends` keyword.

# ⭐ QUICK SUMMARY:

- `__proto__` links objects to their prototype.
- `prototype` is used on functions/constructors to share methods.
- Prototype Chain = property lookup path.
- # Inheritance in JS is prototype-based (classes are sugar).
