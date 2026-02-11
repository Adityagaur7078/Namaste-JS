# Episode 1: How JavaScript Works!

## Execution Context

- Everything in JavaScript happens inside the **execution context**.
- The execution context is like a **big box or container** where the JavaScript code is executed.
- It consists of **two components**:
  - Memory Component (Variable Environment)
  - Code Component (Thread of Execution)

---

## Memory Component (Variable Environment)

- Variables and functions are stored in the **memory component**.
- They are stored as **key-value pairs**.
- The memory component is also known as the **variable environment**.

---

## Code Component (Thread of Execution)

- The code component is where the JavaScript code is executed **line by line**.
- It is also known as the **thread of execution**.

---

## JavaScript Characteristics

JavaScript is a **synchronous single-threaded language**.

### Synchronous
- It executes **one command at a time**.
- Commands are executed in a **specific order**.
- It proceeds to the next line only when the **current line has finished executing**.

### Single-Threaded
- JavaScript can execute **only one command at a time**.










# Episode 2: How JavaScript Code is Executed?

## Execution Context and Phases

- JavaScript code is executed inside **execution contexts**.
- Execution contexts have **two phases**:
  1. Memory Creation Phase
  2. Code Execution Phase

---

## Memory Creation Phase

- In the memory creation phase, **memory is allocated** to variables and functions.
- Variables are assigned the value **undefined**.
- Functions are stored in memory **as they are**.

---

## Code Execution Phase

- In the code execution phase, the code is executed **line by line**.
- Variables created in the Memory Creation Phase start **assigning values** as the code executes.
- Functions are invoked by creating a **new execution context**.
- Every execution context has its **own memory component and code component**.
- Code inside the function is executed.
- `return` statements return control to the **invoking execution context**.
- Return values are stored in memory **if stored in a variable**.
- After the function finishes execution, its **execution context is deleted**.

---

## Execution Context Working

- JavaScript first creates the **Global Execution Context (GEC)**.
- When a function is called, a **new Function Execution Context (FEC)** is created.
- Each execution context contains:
  - Memory Component
  - Code Component
- After execution, control goes back to the previous execution context.

---

## Call Stack

- The **call stack** manages all execution contexts.
- The call stack is also known as:
  - Execution Context Stack
  - Program Stack
  - Control Stack
  - Runtime Stack
  - Machine Stack
- It maintains the **order of execution** of execution contexts.
- Each new execution context is **pushed** onto the stack.
- The **topmost execution context** is the one currently being executed.
- The **bottom execution context** is always the **Global Execution Context (GEC)**.
- Other execution contexts are **Function Execution Contexts (FEC)**.
- When a function finishes execution, its execution context is **popped** from the stack.
- Eventually, the **Global Execution Context is also popped**, and the execution of the program is completed.




# Episode 3: Hoisting in JavaScript!

> **Note:** Everything in this episode is intuitive.
> If you feel any difficulty in understanding, consider watching **Episode 2** first.

---

## What is Hoisting?

Hoisting is a concept in JavaScript that allows variables and function declarations to be accessed **before they are actually defined** in the code.

During the **memory creation phase** of the execution context:

* Variables are initialized with the value **undefined**
* Function declarations are stored in memory **as they are**

---

## Hoisting in Simple Points

* Variables are initialized as **undefined** during the memory allocation phase.
* Function declarations are stored in memory **as they are**.
* Hoisting allows us to:

  * Use variables
  * Call functions
    **before they are declared in the code**
* Using a variable before its declaration does **not throw an error**, but its value will be **undefined** until it is assigned.
* If a variable is **not declared at all**, it is considered **not defined** and accessing it will result in an error.
* Hoisting works differently for:

  * Function declarations
  * Function expressions
  * Arrow function expressions
* Function declarations are **fully hoisted**.
* Function expressions and arrow functions behave like variables and are hoisted with the value **undefined**.

---

## Memory Aid (Easy to Remember)

* Variable declarations are scanned and are made **undefined**
* Function declarations are scanned and are made **available**

---

## Examples of Hoisting

---

### Example 1

```js
getName(); // Namaste Javascript
console.log(x); // undefined

var x = 7;

function getName() {
    console.log('Namaste Javascript');
}
```

**Try to understand by yourself**

#### Technical Language (Use this in Interviews)

* The `getName()` function is called before its declaration, but it works because **function declarations are hoisted**.
* The variable `x` is also hoisted, but it is assigned the value **undefined** until the line `var x = 7` is executed.

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

**Try to understand by yourself**

#### Technical Language (Use this in Interviews)

* The `getName()` function executes successfully because **function declarations are hoisted**.
* The variable `x` throws an error because it is **not declared at all**.
* `console.log(getName)` prints the **entire function definition**, showing that the function is available in memory.

---

### Example 3

```js
getName(); // Uncaught TypeError: getName is not a function
console.log(getName);

var getName = function () {
    console.log('Namaste JavaScript');
};

// The code won't execute as the first line itself throws a TypeError.
```

**Try to understand by yourself**

#### Technical Language (Use this in Interviews)

* Here, `getName` is a **function expression**, not a function declaration.
* During hoisting, `getName` is treated like a variable and is assigned the value **undefined**.
* Calling `getName()` before assignment causes a **TypeError**, because `undefined` is not a function.
* `console.log(getName)` prints **undefined**.
* Execution stops at the first line due to the error, so the function is never assigned.

---

## Important Note

* Function declarations are **fully hoisted**.
* Function expressions and arrow functions are hoisted like variables and get the value **undefined**.
* Understanding this difference is very important for:

  * Debugging
  * Interviews
  * Writing clean JavaScript code









# Episode 4: How Functions Work in JavaScript & Variable Environment

## How Functions Work in JavaScript ❤️

- Functions in JavaScript create their own execution contexts when invoked.
- Each function has its own variable environment (Memory Component), allowing the use of local variables that are scoped within the function.
- Variables declared within a function are accessible only within that function, unless explicitly returned or accessed from an outer scope  
  (This is possible through the concept of closures in JavaScript – will come in later Ep).
- JavaScript uses a process called variable hoisting, which allows functions to be called before they are defined.
- Variable declarations are moved to the top of their respective scopes during the compilation phase.

---

## Variable Environment

- The variable environment (Memory Component) of an execution context is the space where variables and functions are stored during runtime.
- Each execution context has its own variable environment (Memory Component), which holds the variables and functions specific to that context.
- When a variable is accessed, JavaScript searches for its value:
  - First in the local variable environment
  - Then in the outer variable environments
  - Finally in the global variable environment
- This hierarchical structure of variable environments allows for lexical scoping, where variables are resolved based on their proximity to the current execution context.

---

## Code Flow in Terms of Execution Context

```js
var x = 1;
a();
b(); // we are calling the functions before defining them. This will work properly, as seen in Hoisting.
console.log(x);

function a() {
  var x = 10; // local scope because of separate execution context
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



---






# Episode 5: SHORTEST JS Program | window & this keyword!

## JavaScript Shortest Program

- The shortest JavaScript program is an empty file. Although there is no code to execute, the JavaScript engine performs several tasks behind the scenes.

- A global execution context is created, and the global memory component (variable environment) is set up.

- The JavaScript engine creates a global object called "Window" in the browser environment, which contains various functions and variables.

- The global object can be accessed using the Window keyword or this keyword at the global level  
(or At global level, this === window).

- If we create any variable in the global scope, then the variables get attached to the global object.

- In different JavaScript Runtime Environments, the global object may have different names  
(e.g., window in browsers, global in Node.js).

## Code

```js
var x = 10;
console.log(x); // 10
console.log(this.x); // 10
console.log(window.x); // 10
```

## Extra Gyaan (Knowledge)

- The global object is unique and shared across all execution contexts within the same JavaScript environment (e.g., browser or Node.js).
- However, within a function execution context, there is a local object known as the variable object or activation object.
- This local object is created specifically for the execution context of the function.
- It contains local variables, function arguments, and function declarations within that scope.
- In JavaScript, there is no direct or explicit access to the activation context (variable object) from outside the execution context itself.









# Episode 6: undefined vs not defined in JS

## Memory Allocation in JavaScript

- JavaScript allocates memory to variables and functions before executing any code.

- E- ven before a line of code is run, memory space is reserved for variables.

- The value of a variable that hasn't been assigned is `undefined`.

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






# Episode 7: The Scope Chain, Scope & Lexical Environment

## Scope and Lexical Environment

Scope in JavaScript is directly related to **Lexical Environment**.

Let’s observe the below examples:

---

# CASE 1

```js
function a() {
  console.log(b); // 10
  // Instead of printing undefined it prints 10,
  // So somehow this function could access the variable b outside the function scope.
}
var b = 10;
a();
```

# CASE 2

```js
function a() {
  c();
  function c() {
    console.log(b); // 10
  }
}
var b = 10;
a();
```

# CASE 3

```js
function a() {
  c();
  function c() {
    var b = 100;
    console.log(b); // 100
  }
}
var b = 10;
a();
```

# CASE 4

```js
function a() {
  var b = 10;
  c();
  function c() {
    console.log(b); // 10
  }
}
a();
console.log(b); // Error, Not Defined
```

## Understanding the Output

- **Case 1:** Function `a` is able to access variable `b` from the global scope.
- **Case 2:** `10` is printed. It means that within nested functions also, the global scope variable can be accessed.
- **Case 3:** `100` is printed, meaning the local variable of the same name takes precedence over the global variable.
- **Case 4:** A function can access variables from its outer scope, but the global execution context cannot access any local variable.

---

## Execution Context Summary

To summarize the above points in terms of execution context:

```text
call_stack = [GEC, a(), c()]
Now let’s assign the memory sections of each execution context in the call stack:
c()  = [[lexical environment pointer pointing to a()]]
a()  = [b: 10, c: {}, [lexical environment pointer pointing to GEC]]
GEC  = [a: {}, [lexical environment pointer pointing to null]]
```
![lexical-36274f688c07c76d522821c48e1180a4](https://github.com/user-attachments/assets/9c54897f-66a9-4a66-8e17-f450a9531b03)
![lexical2-5dc78ead643a80b62dbc46552330144f](https://github.com/user-attachments/assets/ee6ce6c8-4923-48b6-bb42-1b7e7af21ae6)

## Lexical Environment and Scope Chain

So, **Lexical Environment = local memory + lexical environment of its parent**.  
Hence, **Lexical Environment** is the local memory along with the lexical environment of its parent.

**Lexical** means **in hierarchy, in order**.

Whenever an **Execution Context** is created, a **Lexical Environment (LE)** is also created and is referenced in the local Execution Context (in memory space).

The process of going one by one to the parent and checking for values is called the **scope chain** or **lexical environment chain**.

---

## Lexical Scope Example

```js
function a() {
  function c() {
    // logic here
  }
  c(); // c is lexically inside a
}
// a is lexically inside global execution
```
# Lexical or Static Scope
Lexical or Static scope refers to the accessibility of variables, functions, and objects based on their physical location in the source code.

```js
Global {
  Outer {
    Inner
  }
}
// Inner is surrounded by lexical scope of Outer
```

# TLDR

An inner function can access variables which are in outer functions even if the inner function is nested deep.

In any other case, a function can't access variables not in its scope.
