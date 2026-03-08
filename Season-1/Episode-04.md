# Episode 4: How Functions Work in JavaScript & Variable Environment

## How Functions Work in JavaScript ❤️

- Functions create their own execution contexts when invoked.
- Each function has its own variable environment (Memory Component), allowing local scope.
- Variables declared inside a function are accessible only inside it, unless returned or captured through closures.
- JavaScript uses hoisting, which allows function declarations to be called before definition.
- Variable declarations are moved to the top of their scope during compilation.

---

## Variable Environment

- The variable environment is where variables and functions are stored at runtime.
- Each execution context has its own variable environment.
- When a variable is accessed, JavaScript searches:
  - Local variable environment
  - Outer variable environments
  - Global variable environment
- This hierarchy enables lexical scoping.

---

## Code Flow in Terms of Execution Context

```js
var x = 1;
a();
b(); // Calling before definition works due to hoisting.
console.log(x);

function a() {
  var x = 10;
  console.log(x);
}

function b() {
  var x = 100;
  console.log(x);
}

// Output
// 10
// 100
// 1
```

---

## Explanation of Image

- The code begins with the declaration `var x = 1;`.  
  This creates a variable `x` in the global execution context with the initial value of `1`.

- Next, we encounter the function invocation `a();`.  
  Since functions in JavaScript create their own execution contexts, a new execution context is created for the function `a()`.

- Within the execution context of `a()`, we see the declaration `var x = 10;`.  
  This creates a separate variable `x` with the initial value of `10` in its own variable environment (Memory Component), which is scoped locally within the function `a()`.

- The statement `console.log(x);` within `a()` outputs the value of the local `x`, which is `10`.

- After `a()` finishes executing, its execution context is popped off the call stack, and we return to the global execution context.

- The function invocation `b();` is encountered.  
  Similar to `a()`, a new execution context is created for `b()`.

- Within the execution context of `b()`, we have the declaration `var x = 100;`, creating a separate variable `x` with the value `100` within the local scope of `b()`.

- The statement `console.log(x);` within `b()` outputs the value of the local `x`, which is `100`.

- Once `b()` completes execution, its execution context is popped off the call stack, and we return to the global execution context.

- Finally, we encounter the statement `console.log(x);` within the global scope.  
  Since there is no local `x` variable in this scope, JavaScript accesses the global `x` variable declared earlier, which has a value of `1`.

- The value of `x` is outputted as `1` to the console.


## Execution Context & Variable Environment Diagram

<img width="1083" height="922" alt="diagram" src="https://github.com/user-attachments/assets/0148e0b0-14a2-489b-a8b2-27c183cd9920" />
