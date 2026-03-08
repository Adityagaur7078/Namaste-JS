# Episode 16 : JS Engine Exposed, Google's V8 Architecture

---

* JS runs literally everywhere from smart watch to robots to browsers because of Javascript Runtime Environment (JRE).

* JRE is like a big container which has everything which are required to run Javascript code.

* JRE consists of a JS Engine (?? of JRE), set of APIs to connect with outside environment, event loop, Callback queue, Microtask queue etc.

* Browser can execute javascript code because it has the Javascript Runtime Environment.

* ECMAScript is a governing body of JS. It has set of rules which are followed by all JS engines like Chakra(Edge), Spidermonkey(Firefox)(first javascript engine created by JS creator himself), v8(Chrome)

* Javascript Engine is not a machine. Its software written in low level languages (eg. C++) that takes in hi-level code in JS and spits out low level machine code.

---

## Code inside Javascript Engine passes through 3 steps : **Parsing, Compilation** and **Execution**

### i. Parsing

- Code is broken down into tokens. In "let a = 7" -> let, a, =, 7 are all tokens. Also we have a syntax parser that takes code and converts it into an AST (Abstract Syntax Tree) which is a JSON with all key values like type, start, end, body etc (looks like package.json but for a line of code in JS. Kinda unimportant)(Check out astexplorer.net -> converts line of code into AST).

---

### ii. Compilation

- JS has something called Just-in-time(JIT) Compilation - uses both interpreter & compiler. Also compilation and execution both go hand in hand. The AST from previous step goes to interpreter which converts hi-level code to byte code and moves to execeution. While interpreting, compiler also works hand in hand to compile and form optimized code during runtime. Does JavaScript really Compiles? The answer is a loud YES. JS used to be only interpreter in old times, but now has both to compile and interpreter code and this make JS a JIT compiled language, its like best of both world.

---

### iii. Execution

- Needs 2 components ie. Memory heap(place where all memory is stored) and Call Stack(same call stack from prev episodes). There is also a garbage collector. It uses an algo called **Mark and Sweep**.

![jsengine-6902ed7409b617c6b9c0b8813ac6619a](https://github.com/user-attachments/assets/58482838-e8e8-4a54-8ca2-8471c983bc6f)
![jsenginegif-08a5f0882879cec7e21ab62c123e2afe](https://github.com/user-attachments/assets/d7fba9cd-7da1-4934-8622-721b7b641ff7)

---

* Companies use different JS engines and each try to make theirs the best.

  * v8 of Google has Interpreter called Ignition, a compiler called Turbo Fan and garbage collector called Orinoco

* v8 architecture:

<img width="640" height="480" alt="jsengine-423c0e480647e72e3055af3f385e5f1a" src="https://github.com/user-attachments/assets/a9aecb24-da34-4cc5-99ef-ff2dbdb5eec1" />

---
