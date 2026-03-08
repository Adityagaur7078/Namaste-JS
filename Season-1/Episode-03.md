# Episode 3: Hoisting in JavaScript!

## What Is Hoisting?

Hoisting is a concept in JavaScript that allows variables and function declarations to be accessed **before they are defined** in code.

During the **memory creation phase** of execution context:

- Variables are initialized with **undefined**.
- Function declarations are stored in memory **as they are**.

---

## Hoisting in Simple Points

- Variables are initialized as **undefined** during memory allocation.
- Function declarations are stored in memory **as they are**.
- Hoisting allows us to:
  - Use variables
  - Call functions
  **before they are declared in code**
- Using a variable before declaration does **not throw an error**, but value remains **undefined** until assignment.
- If a variable is **not declared at all**, it is **not defined**, and access throws an error.
- Hoisting works differently for:
  - Function declarations
  - Function expressions
  - Arrow function expressions
- Function declarations are **fully hoisted**.
- Function expressions and arrow functions behave like variables and are hoisted with value **undefined**.

---

## Memory Aid

- Variable declarations are scanned and made **undefined**.
- Function declarations are scanned and made **available**.

---

## Examples of Hoisting

### Example 1

```js
getName(); // Namaste Javascript
console.log(x); // undefined

var x = 7;

function getName() {
  console.log('Namaste Javascript');
}
```

#### Technical Language (Use in Interviews)

- `getName()` is called before declaration, but works because **function declarations are hoisted**.
- `x` is hoisted, but remains **undefined** until `var x = 7` executes.

---

### Example 2

```js
getName(); // Namaste JavaScript
console.log(x); // Uncaught ReferenceError: x is not defined
console.log(getName); // f getName(){ console.log("Namaste JavaScript"); }

function getName() {
  console.log('Namaste JavaScript');
}
```

#### Technical Language (Use in Interviews)

- `getName()` executes because **function declarations are hoisted**.
- `x` throws an error because it is **not declared at all**.
- `console.log(getName)` prints the **entire function definition**.

---

### Example 3

```js
getName(); // Uncaught TypeError: getName is not a function
console.log(getName);

var getName = function () {
  console.log('Namaste JavaScript');
};
```

#### Technical Language (Use in Interviews)

- `getName` here is a **function expression**, not declaration.
- During hoisting, `getName` is treated as variable and set to **undefined**.
- Calling `getName()` before assignment causes **TypeError** (`undefined` is not a function).
- Execution stops at first line.

---

## Important Note

- Function declarations are **fully hoisted**.
- Function expressions and arrow functions are hoisted like variables with value **undefined**.
- This difference is important for **debugging**, **interviews**, and **clean JavaScript code**.
