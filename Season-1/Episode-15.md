# Episode 15 : Asynchronous JavaScript & EVENT LOOP from scratch

> Note: Call stack will execute any execution context which enters it. Time, tide and JavaScript waits for none. TLDR; Call stack has no timer.

---

* Browser has JS Engine which has Call Stack which has Global execution context, local execution context etc.
* But browser has many other superpowers - Local storage space, Timer, place to enter URL, Bluetooth access, Geolocation access and so on.
* Now JS needs some way to connect the callstack with all these superpowers. This is done using Web APIs.

![eventloop1-5daea0168059d1d535f5f7038a739dba](https://github.com/user-attachments/assets/fe45695b-ac02-4021-a1ff-0af0c7b4aa3c)

---

## WebAPIs

None of the below are part of JavaScript! These are extra superpowers that browser has. Browser gives access to JS callstack to use these powers.

![eventloop2-a32e841c3d1d3c52669c3454f4ba99eb](https://github.com/user-attachments/assets/58e759d6-2e52-4aa7-945f-262630fd338f)

- setTimeout(), DOM APIs, fetch(), localstorage, console (yes, even console.log is not JS!!), location and so many more.

  * setTimeout() : Timer function

  * DOM APIs : eg.Document.xxxx ; Used to access HTML DOM tree. (Document Object Manipulation)

  * fetch() : Used to make connection with external servers eg. Netflix servers etc.

- We get all these inside call stack through global object ie. window

  * Use window keyword like : window.setTimeout(), window.localstorage, window.console.log()

  * As window is global obj, and all the above functions are present in global object, we don't explicitly write window but it is implied.

---

## Let's understand the below code and its explanation:

![eventloop3-70f56aaeaad17ca89fde956a74ee97fa](https://github.com/user-attachments/assets/63645567-2af8-4ad1-abad-8bdb16d6ca2e)

```js
console.log('start');
setTimeout(function cb() {
  console.log('timer');
}, 5000);
console.log('end');
// start end timer
```

* First a GEC is created and put inside call stack.
* console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console.
* setTimeout(function cb() { //this calls the setTimeout web api which gives access to timer feature. It stores the callback cb() and starts timer. console.log("Callback");}, 5000);
* console.log("End"); // calls console api and logs in console window. After this GEC pops from call stack.
* While all this is happening, the timer is constantly ticking. After it becomes 0, the callback cb() has to run.
* Now we need this cb to go into call stack. Only then will it be executed. For this we need event loop and **Callback queue**.

---

## Event Loops and Callback Queue

Q: How after 5 secs timer is console?

* cb() cannot simply directly go to callstack to be executed.
* It goes through the callback queue when timer expires.
* Event loop keep checking the callback queue, and see if it has any element to puts it into call stack. It is like a gate keeper.
* Once cb() is in callback queue, eventloop pushes it to callstack to run. Console API is used and log printed

![eventloop4-8e04fda51ca18d88b3843e6e16ee4d2f](https://github.com/user-attachments/assets/60951ed9-1578-4450-a51c-9080b8bfb420)

---

## Another example to understand Eventloop & Callback Queue.
See the below Image and code and try to understand the reason:

![eventloop5-c490492e88d4f0a810e3cd2e8d202080](https://github.com/user-attachments/assets/535fe9ee-79b8-4029-a187-958737b6cbe3)

Explaination?
```js
console.log('Start');
document.getElementById('btn').addEventListener('click', function cb() {
  // cb() registered inside webapi environment and event(click) attached to it. i.e. REGISTERING CALLBACK AND ATTACHING EVENT TO IT.
  console.log('Callback');
});
console.log('End'); // calls console api and logs in console window. After this GEC get removed from call stack.
```

* In above code, even after console prints "Start" and "End" and pops GEC out, the eventListener stays in webapi env(with hope that user may click it some day) until explicitly removed, or the browser is closed.
* Eventloop has just one job to keep checking callback queue and if found something push it to call stack and delete from callback queue.

---

## Q: Need of callback queue?

Ans: Suppose user clciks button x6 times. So 6 cb() are put inside callback queue. Event loop sees if call stack is empty/has space and whether callback queue is not empty(6 elements here). Elements of callback queue popped off, put in callstack, executed and then popped off from call stack.

---

## Behaviour of fetch (Microtask Queue?)

```js
console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console.
setTimeout(function cbT() {
  console.log("CB Timeout");
}, 5000);
fetch("https://api.netflix.com").then(function cbF() {
    console.log("CB Netflix");
}); // take 2 seconds to bring response
// millions lines of code
console.log("End");
```

Code Explaination:

* Same steps for everything before fetch() in above code.
* fetch registers cbF into webapi environment along with existing cbT.
* cbT is waiting for 5000ms to end so that it can be put inside callback queue. cbF is waiting for data to be returned from Netflix servers gonna take 2 seconds.
* After this millions of lines of code is running, by the time millions line of code will execute, 5 seconds has finished and now the timer has expired and response from Netflix server is ready.
* Data back from cbF ready to be executed gets stored into something called a Microtask Queue.
* Also after expiration of timer, cbT is ready to execute in Callback Queue.
* Microtask Queue is exactly same as Callback Queue, but it has higher priority. Functions in Microtask Queue are executed earlier than Callback Queue.
* In console, first Start and End are printed in console. First cbF goes in callstack and "CB Netflix" is printed. cbF popped from callstack. Next cbT is removed from callback Queue, put in Call Stack, "CB Timeout" is printed, and cbT removed from callstack.
* See below Image for more understanding.

![eventloop6-f8b232d201a82777b680dffbf5d3ccdf](https://github.com/user-attachments/assets/feeaa979-53fa-497f-88cd-da446f4df1c0)

---
## Microtask Priority Visualization

![microtask-64675cdce0c256cdfebffdc3cac60016](https://github.com/user-attachments/assets/10793736-778d-454b-9f2f-fb33af7945b5)

## What enters the Microtask Queue ?

* All the callback functions that come through promises go in microtask Queue.
* **Mutation Observer** : Keeps on checking whether there is mutation in DOM tree or not, and if there, then it executes some callback function.
* Callback functions that come through promises and mutation observer go inside **Microtask Queue**.
* All the rest goes inside **Callback Queue aka. Task Queue**.
* If the task in microtask Queue keeps creating new tasks in the queue, element in callback queue never gets chance to be run. This is called starvation

---

## Some Important Questions

1. **When does the event loop actually start ?** - Event loop, as the name suggests, is a single-thread, loop that is almost infinite. It's always running and doing its job.

2. **Are only asynchronous web api callbacks are registered in web api environment?** - YES, the synchronous callback functions like what we pass inside map, filter and reduce aren't registered in the Web API environment. It's just those async callback functions which go through all this.

3. **Does the web API environment stores only the callback function and pushes the same callback to queue/microtask queue?** - Yes, the callback functions are stored, and a reference is scheduled in the queues. Moreover, in the case of event listeners(for example click handlers), the original callbacks stay in the web API environment forever, that's why it's adviced to explicitly remove the listeners when not in use so that the garbage collector does its job.

4. **How does it matter if we delay for setTimeout would be 0ms. Then callback will move to queue without any wait ?** - No, there are trust issues with setTimeout() ??. The callback function needs to wait until the Call Stack is empty. So the 0 ms callback might have to wait for 100ms also if the stack is busy.

---

## Observation of Eventloop, Callback Queue & Microtask Queue [GiF]

![microtask1-d3aad8c29c0a29ac35ee2dac26c8cd57](https://github.com/user-attachments/assets/1576d82e-7103-4d32-ad12-a1aeb3bb4c17)
![microtask2-5b8a5d1ff9a7257b3f7818a43ad7bd18](https://github.com/user-attachments/assets/435030a2-f5e7-4fa8-864e-e99d492a4ec2)
![microtask3-a2c6b052fe90284a88f725ac0037a3f1](https://github.com/user-attachments/assets/6a0b8dea-ad8e-4b2f-9c81-284eb0f0fbdd)
![microtask4-934ae4c9468b589dad16549b47403751](https://github.com/user-attachments/assets/cfc73eee-8fb3-42e8-9f28-d2d0fed05df4)
![microtask5-a3cdebd52874eb1b0d19dca5dee81bf6](https://github.com/user-attachments/assets/268efc32-4121-4b38-9097-defdb7b1e3da)
![microtask6-5d251829eb7a4b403f6910fed70828f6](https://github.com/user-attachments/assets/ec06ac65-8261-48f4-8e7e-1b7f650d079c)

---
