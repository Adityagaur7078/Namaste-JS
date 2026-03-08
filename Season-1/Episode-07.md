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
