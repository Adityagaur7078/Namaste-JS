# Episode 3 : Creating a Promise, Chaining & Error Handling

---

```js
const cart = ['shoes', 'pants', 'kurta'];

// Consumer part of promise
const promise = createOrder(cart); // orderId
// Our expectation is above function is going to return me a promise.

promise.then(function (orderId) {
  proceedToPayment(orderId);
});
```

Above snippet we have observed in our previous lecture itself.
Now we will see, how createOrder is implemented so that it is returning a promise
In short we will see, "How we can create Promise" and then return it.

---

## Producer part of Promise

```js
function createOrder(cart) {
  // JS provides a Promise constructor through which we can create promise
  // It accepts a callback function with two parameter `resolve` & `reject`
  const promise = new Promise(function (resolve, reject) {
    // What is this `resolve` and `reject`?
    // These are function which are passed by javascript to us in order to handle success and failure of function call.

    /** Mock logic steps
     * 1. validateCart
     * 2. Insert in DB and get an orderId
     */

    if (!validateCart(cart)) {
      // If cart not valid, reject the promise
      const err = new Error('Cart is not Valid');
      reject(err);
    }

    const orderId = '12345'; // We got this id by calling to db (Assumption)
    if (orderId) {
      // Success scenario
      resolve(orderId);
    }
  });
  return promise;
}
```

---

Over above, if your validateCart is returning true, so the above promise will be resolved (success)

```js
const cart = ['shoes', 'pants', 'kurta'];

const promise = createOrder(cart); // orderId

// ? What will be printed in below line?
// It prints Promise {<pending>}, but why?
// Because above createOrder is going to take sometime to get resolved, so pending state.
// But once the promise is resolved, `.then` would be executed for callback.
console.log(promise);

promise.then(function (orderId) {
  proceedToPayment(orderId);
});
```

---

## Catching Errors using `.catch`

```js
const cart = ['shoes', 'pants', 'kurta'];

const promise = createOrder(cart); // orderId

promise
  .then(function (orderId) {
    // ? success aka resolved promise handling
    proceedToPayment(orderId);
  })
  .catch(function (err) {
    // ?? failure aka reject handling
    console.log(err);
  });
```

---

## Promise Chaining

For this we will assume after createOrder we have to invoke proceedToPayment

In promise chaining, whatever is returned from first `.then` become data for next `.then` and so on...

At any point of promise chaining, if promise is rejected, the execution will fallback to `.catch` and others promise won't run.

```js
const cart = ['shoes', 'pants', 'kurta'];

createOrder(cart)
  .then(function (orderId) {
    // ? success aka resolved promise handling
    proceedToPayment(orderId);
    return orderId;
  })
  .then(function (orderId) {
    // Promise chaining
    // ?? we will make sure that `proceedToPayment` returns a promise too
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    // from above, `proceedToPayment` is returning a promise so we can consume using `.then`
    console.log(paymentInfo);
  })
  .catch(function (err) {
    // ?? failure aka reject handling
    console.log(err);
  });
```

---

## proceedToPayment Implementation

```js
function proceedToPayment(cart) {
  return new Promise(function (resolve, reject) {
    // For time being, we are simply `resolving` promise
    resolve('Payment Successful');
  });
}
```

---

## Continuing Execution Even After Failure

Q: What if we want to continue execution even if any of my promise is failing, how to achieve this?

-> By placing the `.catch` block at some level after which we are not concerned with failure.

-> There could be multiple `.catch` too.

```js
createOrder(cart)
  .then(function (orderId) {
    proceedToPayment(orderId);
    return orderId;
  })
  .catch(function (err) {
    // ?? Whatever fails below it, catch wont care
    // this block is responsible for code block above it.
    console.log(err);
  })
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    console.log(paymentInfo);
  });
```

---
