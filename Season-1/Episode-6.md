# Episode 6: undefined vs not defined in JS

## Memory Allocation in JavaScript

- JavaScript allocates memory to variables/functions before execution.
- Even before a line runs, memory space is reserved for variables.
- Unassigned variables get `undefined`.
- `undefined` acts as a placeholder or default value in memory until a variable is assigned a different value.

---

## Difference between undefined and not defined

- `undefined` means that memory has been allocated to a variable but no value has been assigned yet.
- `not defined` refers to a variable that has not been declared or allocated any memory.

---

## undefined vs Empty

- `undefined` is not equivalent to empty or null.
- It is a special keyword in JavaScript that takes up its own memory space.
- `undefined` is a placeholder until a value is assigned to a variable.

---

## Examples of undefined

```js
// Example 1
var a; // Memory is allocated for 'a', but no value is assigned yet
console.log(a); // Output: undefined

// Example 2
var x;
console.log(x); // Output: undefined

// Example 3
console.log(y); // Output: ReferenceError: y is not defined
```
## JavaScript is a Loosely Typed / Weakly Typed Language

- JavaScript is a loosely typed / weakly typed language.

- It doesn't attach variables to any datatype.

We can say:

```js
var a = 5;
a = true;
a = 'hello';
```