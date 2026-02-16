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

  ### CASE 1

    ```js
    function a() {
      console.log(b); // 10
      // Instead of printing undefined it prints 10,
      // So somehow this function could access the variable b outside the function scope.
    }
    var b = 10;
    a();
    ```

  ### CASE 2

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

  ### CASE 3

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

  ### CASE 4

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
## Lexical or Static Scope
Lexical or Static scope refers to the accessibility of variables, functions, and objects based on their physical location in the source code.

```js
Global {
  Outer {
    Inner
  }
}
// Inner is surrounded by lexical scope of Outer
```

## TLDR

An inner function can access variables which are in outer functions even if the inner function is nested deep.

In any other case, a function can't access variables not in its scope.









# Episode 8: let & const in JS, Temporal Dead Zone

let and const declarations are hoisted. But it is different from var.

```js
console.log(a); // ReferenceError: Cannot access 'a' before initialization
console.log(b); // prints undefined as expected

let a = 10;
console.log(a); // 10

var b = 15;

console.log(window.a); // undefined
console.log(window.b); // 15
```

It looks like `let` isn't hoisted, but it is. Let's understand.

- Both `a` and `b` are actually initialized as `undefined` in the hoisting stage. But `var b` is inside the storage space of **GLOBAL**, and `a` is in a separate memory object called **script**, where it can be accessed only after assigning some value to it first. Thus, one can access `a` only if it is assigned. That, it throws an error.

---

## Temporal Dead Zone
**Temporal Dead Zone :** Time since when the let variable was hoisted until it is initialized some value.

  - So any line till before "let a = 10" is the TDZ for a

  - Since a is not accessible on global, its not accessible in window/this also. window.b or this.b -> 15; But window.a or this.a ->undefined, just like window.x->undefined (x isn't declared anywhere)

**Reference Error** are thrown when variables are in temporal dead zone.

**Syntax Error** doesn't even let us run single line of code.

- ```js
  let a = 10;
  let a = 100;  //this code is rejected upfront as SyntaxError. (duplicate declaration)
  ------------------
  let a = 10;
  var a = 100; // this code also rejected upfront as SyntaxError. (can't use same name in same scope)
  ```

- **Let** is a stricter version of **var**. Now, **const** is even more stricter than **let**.

  ```js
  let a;
  a = 10;
  console.log(a) // 10. Note declaration and assigning of a is in different lines.
  ------------------
  const b;
  b = 10;
  console.log(b); // SyntaxError: Missing initializer in const declaration. (This type of declaration won't work with const. const b = 10 only will work)
  ------------------
  const b = 100;
  b = 1000; //this gives us TypeError: Assignment to constant variable.
  ```

## Types of Error

- **Types of Error:** Syntax, Reference, and Type.

  - **Uncaught ReferenceError: x is not defined at ...**

    - This Error signifies that x has never been in the scope of the program. This literally means that x was never defined/declared and is being tried to be accessed.

  - **Uncaught ReferenceError: cannot access 'a' before initialization**

    - This Error signifies that 'a' cannot be accessed because it is declared as 'let' and since it is not assigned a value, it is its Temporal Dead Zone. Thus, this error occurs.

  - **Uncaught SyntaxError: Identifier 'a' has already been declared**

    - This Error signifies that we are redeclaring a variable that is 'let' declared. No execution will take place.

  - **Uncaught SyntaxError: Missing initializer in const declaration**

    - This Error signifies that we haven't initialized or assigned value to a const declaration.

  - **Uncaught TypeError: Assignment to constant variable**

    - This Error signifies that we are reassigning to a const variable.

---

## SOME GOOD PRACTICES

- Try using **const** wherever possible.

- If not, use **let**, Avoid **var**.

- Declare and initialize all variables with **let** at the top to avoid errors and shrink the Temporal Dead Zone window to zero.

---








# Episode 9 : Block Scope & Shadowing in JS

What is a **Block**?

  - Block aka compound statement is used to group JS statements together into 1 group. We group them within {...}
    
    ```js
    {
      var a = 10;
      let b = 20;
      const c = 30;
      // Here let and const are hoisted in Block scope,
      // While, var is hoisted in Global scope.
    }
    ```

  - Block Scope and its accessibility example

    ```js
    {
      var a = 10;
      let b = 20;
      const c = 30;
    }
    console.log(a); // 10
    console.log(b); // Uncaught ReferenceError: b is not defined
    ```

    ## Reason?

- In the BLOCK SCOPE; we get b and c inside it initialized as *undefined* as a part of hoisting (in a seperate memory space called **block**)

- While, a is stored inside a GLOBAL scope.

- Thus we say, **let and const** are BLOCK SCOPED.
  - They are stored in a separate mem space which is reserved for this block.
  - Also, they can't be accessed outside this block.
  - But var a can be accessed anywhere as it is in global scope.
  - Thus, we can't access them outside the Block.

---

## What is Shadowing?
  ```js
  var a = 100;
  {
    var a = 10; // same name as global var
    let b = 20;
    const c = 30;
    console.log(a); // 10
    console.log(b); // 20
    console.log(c); // 30
  }
  console.log(a); // 10, instead of the 100 we were expecting. So block "a" modified val of global "a" as well. In console, only b and c are in block space. a initially is in global space(a = 100), and when a = 10 line is run, a is not created in block space, but replaces 100 with 10 in global space itself.
  ```

- So, If one has same named variable outside the block, the variable inside the block shadows the outside variable. This happens only for var
  
- Let's observe the behaviour in case of let and const and understand it's reason.

  ```js
  let b = 100;
  {
    var a = 10;
    let b = 20;
    const c = 30;
    console.log(b); // 20
  }
  console.log(b); // 100, Both b's are in separate spaces (one in Block(20) and one in Script(another arbitrary mem space)(100)). Same is also true for *const* declarations.
  ```

![scope-b55ae71a98857349350a52adaeeccf17](https://github.com/user-attachments/assets/64002d7e-a567-4f7c-a8ee-d132654c44df)


- Same logic is true even for **functions**
  
  ```js
  const c = 100;
  function x() {
    const c = 10;
    console.log(c); // 10
  }
  x();
  console.log(c); // 100
  ```

## What is **Illegal Shadowing**?

  ```js
  let a = 20;
  {
    var a = 20;
  }
  // Uncaught SyntaxError: Identifier 'a' has already been declared
  ```

- We cannot shadow let with var. But it is valid to shadow a let using a let. However, we can shadow var with let.
- All scope rules that work in function are same in arrow functions too.
- Since var is function scoped, it is not a problem with the code below.

```js
let a = 20;
function x() {
  var a = 20;
}
```
---







# Episode 10 : Closures in JS

## What is Closure?

- Function bundled along with it's lexical scope is closure.

- JavaScript has a lexcial scope environment. If a function needs to access a variable, it first goes to its local       memory. When it does not find it there, it goes to the memory of its lexical parent. See Below code, Over here function y along with its lexical scope i.e. (function x) would be called a closure.

```js
function x() {
  var a = 7;
  function y() {
    console.log(a);
  }
  return y;
}
var z = x();
console.log(z); // value of z is entire code of function y.
```

  - In above code, When y is returned, not only is the function returned but the entire closure (fun y + its lexical scope) is returned and put inside z. So when z is used somewhere else in program, it still remembers var a inside x()

- Another Example

```js
function z() {
  var b = 900;
  function x() {
    var a = 7;
    function y() {
      console.log(a, b);
    }
    y();
  }
  x();
}
z(); // 7 900
```

- Thus In simple words, we can say:
  - **A closure is a function** that has access to its outer function scope even after the function has returned. Meaning, A closure can remember and access variables and arguments reference of its outer function even after the function has returned.

![closure-0b8cc2705d6209464e867f271a00c6ac](https://github.com/user-attachments/assets/df0b6b25-b622-44e0-a50c-e021060adbe2)


- Advantages of Closure:
  - Module Design Pattern
  - Currying
  - Memoize
  - Data hiding and encapsulation
  - setTimeouts etc.

- Disadvantages of Closure:
  
  - Over consumption of memory
  - Memory Leak
  - Freeze browser
  
---







# Episode 11 : setTimeout + Closures Interview Question

> **Time, tide and Javascript wait for none**.

```js
function x() {
  var i = 1;
  setTimeout(function () {
    console.log(i);
  }, 3000);
  console.log('Namaste Javascript');
}
x();
```

## Output:

```
Namaste Javascript
1 // after waiting 3 seconds
```

* We expect JS to wait 3 sec, print 1 and then go down and print the string. But JS prints string immediately, waits 3 sec and then prints 1.
* The function inside setTimeout forms a closure (remembers reference to i). So wherever function goes it carries this ref along with it.
* setTimeout takes this callback function & attaches timer of 3000ms and stores it. Goes to next line without waiting and prints string.
* After 3000ms runs out, JS takes function, puts it into call stack and runs it.

---

## Q: Print 1 after 1 sec, 2 after 2 sec till 5 : Tricky interview question

We assume this has a simple approach as below

```js
function x() {
  for (var i = 1; i <= 5; i++) {
    setTimeout(function () {
      console.log(i);
    }, i * 1000);
  }
  console.log('Namaste Javascript');
}
x();
```

## Output:

```
Namaste Javascript
6
6
6
6
6
```

* Reason?

  * This happens because of closures. When setTimeout stores the function somewhere and attaches timer to it, the function remembers its reference to i, not value of i. All 5 copies of function point to same reference of i. JS stores these 5 functions, prints string and then comes back to the functions. By then the timer has run fully. And due to looping, the i value became 6. And when the callback fun runs the variable i = 6. So same 6 is printed in each log

  * To avoid this, we can use let instead of var as let has Block scope. For each iteration, the i is a new variable altogether(new copy of i). Everytime setTimeout is run, the inside function forms closure with new variable i

* But what if interviewer ask us to implement using var?

```js
function x() {
  for (var i = 1; i <= 5; i++) {
    function close(i) {
      setTimeout(function () {
        console.log(i);
      }, i * 1000);
      // put the setT function inside new function close()
    }
    close(i); // everytime you call close(i) it creates new copy of i. Only this time, it is with var itself!
  }
  console.log('Namaste Javascript');
}
x();
```
---






# Episode 12 : Famous Interview Questions ft. Closures

---

## Q1: What is Closure in Javascript?

**Ans:** A function along with reference to its outer environment together forms a closure. Or in other words, A Closure is a combination of a function and its lexical scope bundled together. eg:

```js
function outer() {
  var a = 10;
  function inner() {
    console.log(a);
  } // inner forms a closure with outer
  return inner;
}
outer()(); // 10 // over here first `()` will return inner function and then using second `()` to call inner function
```

---

## Q2: Will the below code still forms a closure?

```js
function outer() {
  function inner() {
    console.log(a);
  }
  var a = 10;
  return inner;
}
outer()(); // 10
```

**Ans:** Yes, because inner function forms a closure with its outer environment so sequence doesn't matter.

---

## Q3: Changing var to let, will it make any difference?

```js
function outer() {
  let a = 10;
  function inner() {
    console.log(a);
  }
  return inner;
}
outer()(); // 10
```

**Ans:** It will still behave the same way.

---

## Q4: Will inner function have the access to outer function argument?

```js
function outer(str) {
  let a = 10;
  function inner() {
    console.log(a, str);
  }
  return inner;
}
outer('Hello There')(); // 10 "Hello There"
```

**Ans:** Inner function will now form closure and will have access to both a and str.

---

## Q5: In below code, will inner form closure with outest?

```js
function outest() {
  var c = 20;
  function outer(str) {
    let a = 10;
    function inner() {
      console.log(a, c, str);
    }
    return inner;
  }
  return outer;
}
outest()('Hello There')(); // 10 20 "Hello There"
```

**Ans:** Yes, inner will have access to all its outer environment.

---

## Q6: Output of below code and explanation?

```js
function outest() {
  var c = 20;
  function outer(str) {
    let a = 10;
    function inner() {
      console.log(a, c, str);
    }
    return inner;
  }
  return outer;
}
let a = 100;
outest()('Hello There')(); // 10 20 "Hello There"
```

**Ans:** Still the same output, the inner function will have reference to inner a, so conflicting name won't matter here. If it wouldn't have find a inside outer function then it would have went more outer to find a and thus have printed 100. So, it try to resolve variable in scope chain and if a wouldn't have been found it would have given reference error.

---

## Q7: Advantage of Closure?

* Module Design Pattern
* Currying
* Memoize
* Data hiding and encapsulation
* setTimeouts etc.

---

## Q8: Discuss more on Data hiding and encapsulation?

```js
// without closures
var count = 0;
function increment(){
  count++;
}
// in the above code, anyone can access count and change it.
```

---

```js
// (with closures) -> put everything into a function
function counter() {
  var count = 0;
  function increment(){
    count++;
  }
}
console.log(count); // this will give referenceError as count can't be accessed. So now we are able to achieve hiding of data
```

---

```js
// (increment with function using closure) true function
function counter() {
  var count = 0;
  return function increment(){
    count++;
    console.log(count);
  }
}
var counter1 = counter(); //counter function has closure with count var.
counter1(); // increments counter

var counter2 = counter();
counter2(); // here counter2 is whole new copy of counter function and it wont impact the output of counter1
```

---

```js
// Adding decrement counter and refactoring code:
function Counter() {
  //constructor function. Good coding would be to capitalize first letter of constructor function.
  var count = 0;
  this.incrementCounter = function() { //anonymous function
    count++;
    console.log(count);
  }
  this.decrementCounter = function() {
    count--;
    console.log(count);
  }
}

var counter1 = new Counter();  // new keyword for constructor fun
counter1.incrementCounter();
counter1.incrementCounter();
counter1.decrementCounter();
// returns 1 2 1
```

---

## Q9: Disadvantage of closure?

**Ans:** Overconsumption of memory when using closure as everytime as those closed over variables are not garbage collected till program expires. So when creating many closures, more memory is accumulated and this can create memory leaks if not handled.

**Garbage collector :** Program in JS engine or browser that frees up unused memory. In highlevel languages like C++ or JAVA, garbage collection is left to the programmer, but in JS engine its done implicitly.

```js
function a() {
  var x = 0;
  return function b() {
    console.log(x);
  };
}

var y = a(); // y is a copy of b()
y();

// Once a() is called, its element x should be garbage collected ideally. But fun b has closure over var x. So mem of x cannot be freed. Like this if more closures formed, it becomes an issue. To tackle this, JS engines like v8 and Chrome have smart garbage collection mechanisms.
```
---









# Episode 13 : First Class Functions ft. Anonymous Functions

> Functions are heart ♥ of Javascript.

---

## Q: What is Function statement?

Below way of creating function are function statement.

```js
function a() {
  console.log('Hello');
}

a(); // Hello
```

---

## Q: What is Function Expression?

Assigning a function to a variable. Function acts like a value.

```js
var b = function () {
  console.log('Hello');
};

b();
```

---

## Q: Difference between function statement and expression

The major difference between these two lies in **Hoisting**.

```js
a(); // "Hello A"
b(); // TypeError

function a() {
  console.log('Hello A');
}

var b = function () {
  console.log('Hello B');
};
```

Why?

During memory creation phase `a` is created in memory and function assigned to `a`. But `b` is created like a variable (`b: undefined`) and until code reaches the `function()` part, it is still undefined. So it cannot be called.

---

## Q: What is Function Declaration?

Other name for **function statement**.

---

## Q: What is Anonymous Function?

A function without a name.

```js
function () {

}
// this is going to throw Syntax Error - Function Statement requires function name.
```

* They don't have their own identity. So an anonymous function without code inside it results in an error.
* Anonymous functions are used when functions are used as values eg. the code sample for **function expression** above.

---

## Q: What is Named Function Expression?

Same as Function Expression but function has a name instead of being anonymous.

```js
var b = function xyz() {
  console.log('b called');
};

b(); // "b called"
xyz(); // Throws ReferenceError: xyz is not defined.
// xyz function is not created in global scope. So it can't be called.
```

---

## Q: Parameters vs Arguments?

```js
var b = function (param1, param2) {
  // labels/identifiers are parameters
  console.log('b called');
};

b(arg1, arg2); // arguments - values passed inside function call
```

---

## Q: What is First Class Function aka First Class Citizens?

We can pass functions inside a function as arguments and/or return a function (HOF). These abilities are altogether known as First Class Function. It is programming concept available in some other languages too.

```js
var b = function (param1) {
  console.log(param1); // prints " f() {} "
};

b(function () {});
```

---

### Other way of doing the same thing:

```js
var b = function (param1) {
  console.log(param1);
};

function xyz() {}

b(xyz); // same thing as previous code
```

---

### We can return a function from a function:

```js
var b = function (param1) {
  return function () {};
};

console.log(b()); // we log the entire function returned within b
```

---









# Episode 14 : Callback Functions in JS ft. Event Listeners

---

## Callback Functions

* Functions are first class citizens ie. take a function A and pass it to another function B. Here, A is a callback function. So basically I am giving access to function B to call function A. This callback function gives us the access to whole Asynchronous world in Synchronous world.

```js
setTimeout(function () {
  console.log('Timer');
}, 1000); // first argument is callback function and second is timer.
```

* JS is a synchronous and single threaded language. But due to callbacks, we can do async things in JS.

```js
setTimeout(function () {
  console.log('timer');
}, 5000);

function x(y) {
  console.log('x');
  y();
}

x(function y() {
  console.log('y');
});

// x y timer
```

* In the call stack, first x and y are present. After code execution, they go away and stack is empty. Then after 5 seconds (from beginning) anonymous suddenly appear up in stack ie. setTimeout
* All 3 functions are executed through call stack. If any operation blocks the call stack, its called blocking the main thread.
* Say if x() takes 30 sec to run, then JS has to wait for it to finish as it has only 1 call stack/1 main thread. Never block main thread.
* Always use async for functions that take time eg. setTimeout

---

### Another Example of callback

```js
function printStr(str, cb) {
  setTimeout(() => {
    console.log(str);
    cb();
  }, Math.floor(Math.random() * 100) + 1);
}

function printAll() {
  printStr('A', () => {
    printStr('B', () => {
      printStr('C', () => {});
    });
  });
}

printAll(); // A B C // in order
```

---

## Event Listener

* We will create a button in html and attach event to it.

```js
// index.html
<button id='clickMe'>Click Me!</button>

// in index.js

document.getElementById('clickMe').addEventListener('click', function xyz() {
  // when event click occurs, this callback function (xyz) is called into callstack
  console.log('Button clicked');
});
```

---

### Lets implement a increment counter button.

#### Using global variable (not good as anyone can change it)

```js
let count = 0;

document
  .getElementById('clickMe')
  .addEventListener('click', function xyz() {
    console.log('Button clicked', ++count);
  });
```

---

#### Use closures for data abstraction

```js
function attachEventList() {
  // creating new function for closure
  let count = 0;

  document
    .getElementById('clickMe')
    .addEventListener('click', function xyz() {
      console.log('Button clicked', ++count); // now callback function forms closure with outer scope(count)
    });
}

attachEventList();
```



---

## Garbage Collection and removeEventListeners

* Event listeners are heavy as they form closures. So even when call stack is empty, EventListener won't free up memory allocated to count as it doesn't know when it may need count again.
* So we remove event listeners when we don't need them (garbage collected).
* onClick, onHover, onScroll all in a page can slow it down heavily.

---
