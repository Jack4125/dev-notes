---
title: "Promise"
date: "2020-08-23"
description: "JavaScript Promises"
---

```js
/*
  await is an operator: it returns the resolved value of a promise. 
  Since promises resolve in an indeterminate amount of time, await 
  halts, or pauses, the execution of our async function until a given 
  promise is resolved.
*/
let myPromise = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Yay, I resolved!")
    }, 1000)
  })
}

// const noAwait = async () => {
async function noAwait() {
  let value = myPromise()
  console.log(value)
}

async function yesAwait() {
  let value = await myPromise()
  console.log(value)
}

/*
async functions always return a promise. This means we can use 
traditional promise syntax, like .then() and .catch with our async 
functions. An async function will return in one of three ways:

1. If there’s nothing returned from the function, it will return a 
promise with a resolved value of undefined.
2. If there’s a non-promise value returned from the function, it will 
return a promise resolved to that value.
3. If a promise is returned from the function, it will simply return 
that promise 
*/

/*
Within our async function, asyncFuncExample(), we use await to halt 
our execution until myPromise() is resolved and assign its resolved 
value to the variable resolvedValue. Then we log resolvedValue to the 
console. We’re able to handle the logic for a promise in a way that reads 
like synchronous code. 
*/
noAwait() // Prints: Promise { <pending> }
yesAwait() // Prints: Yay, I resolved!

/*
Without the await keyword, 
the function execution wasn’t paused. The console.log() on the following 
line was executed before the promise had resolved.

Remember that the await operator returns the resolved value of a promise. 
When used properly in yesAwait(), the variable value was assigned the resolved 
value of the myPromise() promise, whereas in noAwait(), value was assigned the 
promise object itself, which was <pending> at the time.
*/

/*
The true beauty of async...await is when we have a series of asynchronous 
actions which depend on one another. For example, we may make a network request 
based on a query to a database. In that case, we would need to wait to make the 
network request until we had the results from the database. With native promise 
syntax, we use a chain of .then() functions making sure to return correctly each one.
*/

// Promise Syntax
function nativePromiseVersion() {
  /**
   * Async operation, can't use declaration syntax 'const variable = ...'
   * because the 'variable' may not get populated by next line
   * That's why it's a promise, so we have to chain .then()
   */
  returnsFirstPromise()
    .then(firstValue => {
      console.log(firstValue)
      return returnsSecondPromise(firstValue)
    })
    .then(secondValue => {
      console.log(secondValue)
    })
}

// Async Await Syntax
async function asyncAwaitVersion() {
  try {
    let firstValue = await returnsFirstPromise()
    console.log(firstValue)
    let secondValue = await returnsSecondPromise(firstValue)
    console.log(secondValue)
  } catch (err) {
    // Catches any errors in the try block
    console.log(err)
  }
}

/*
Remember that await halts the execution of our async function. 
This allows us to conveniently write synchronous-style code to 
handle dependent promises. 
But what if our async function contains multiple promises which 
are not dependent on the results of one another to execute?
*/
async function waiting() {
  const firstValue = await firstAsyncThing()
  const secondValue = await secondAsyncThing()
  console.log(firstValue, secondValue)
}

async function concurrent() {
  const firstPromise = firstAsyncThing()
  const secondPromise = secondAsyncThing()
  console.log(await firstPromise, await secondPromise)
}

/*
  In the waiting() function, we pause our function until the first 
  promise resolves, then we construct the second promise. Once that 
  resolves, we print both resolved values to the console.

  In our concurrent() function, both promises are constructed without 
  using await. We then await each of their resolutions to print them 
  to the console.

  With concurrent() function, both promises’ asynchronous operations 
  can be run simultaneously. If possible, we want to get started on 
  each asynchronous operation as soon as possible! Within our async functions
  we should still take advantage of concurrency, the ability to perform 
  asynchronous actions at the same time.

  Note: if we have multiple truly independent promises that we would like 
  to execute fully in parallel, we must use individual .then() functions 
  and avoid halting our execution with await.

  Another way to take advantage of concurrency when we have multiple promises 
  which can be executed simultaneously is to await a Promise.all().

  We can pass an array of promises as the argument to Promise.all(), and it 
  will return a single promise. This promise will resolve when all of the 
  promises in the argument array have resolved. This promise’s resolve value 
  will be an array containing the resolved values of each promise from the 
  argument array.
 */

async function asyncPromAll() {
  const resultArray = await Promise.all([
    asyncTask1(),
    asyncTask2(),
    asyncTask3(),
    asyncTask4(),
  ])
  for (let i = 0; i < resultArray.length; i++) {
    console.log(resultArray[i])
  }
}

/* 
In our above example, we await the resolution of a Promise.all(). This 
Promise.all() was invoked with an argument array containing four promises 
(returned from required-in functions). 

Promise.all() allows us to take advantage of asynchronicity — each of the four 
asynchronous tasks can process concurrently. Promise.all() also has the benefit 
of failing fast, meaning it won’t wait for the rest of the asynchronous actions 
to complete once any one has rejected. As soon as the first promise in the array 
rejects, the promise returned from Promise.all() will reject with that reason. 
As it was when working with native promises, Promise.all() is a good choice if 
multiple asynchronous tasks are ALL required, but none must wait for any other 
before executing.*/
```
