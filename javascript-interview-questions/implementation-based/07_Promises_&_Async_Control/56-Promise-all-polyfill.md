
---

> [!quote] Metadata  
> **Posted on:** March 14, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Promise.all() Polyfill

Working with promises is not easy until you have a thorough understanding of how it works, thus in many interviews we are asked to implement polyfills for `Promise.all()` method.

As per MDN:
> The `Promise.all()` accepts an array of promises and returns a promise that resolves when all of the promises in the array are fulfilled or when the iterable contains no promises. It rejects with the reason of the first promise that rejects.

---

## Example

```javascript
function task(time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve(time);
    }, time);
  });
}

const taskList = [task(1000), task(5000), task(3000)];

myPromiseAll(taskList)
  .then(results => {
    console.log("got results", results);
  })
  .catch(console.error);

// Output: "got results" [1000, 5000, 3000]
```

---

## Polyfill for Promise.all()

After reading the definition of `Promise.all()` we can break down the problem into sub-problems:
1. It will return a promise.
2. The promise will **resolve** with result of all the passed promises or **reject** with the error message of first failed promise.
3. The results are returned in the same order as the promises are in the given array.

```javascript
function myPromiseAll(taskList) {
  // to store results
  const results = [];
  // to track how many promises have completed
  let promisesCompleted = 0;

  // return new promise
  return new Promise((resolve, reject) => {
    taskList.forEach((promise, index) => {
      // if promise passes
      promise
        .then((val) => {
          // store its outcome and increment the count
          results[index] = val;
          promisesCompleted += 1;

          // if all the promises are completed,
          // resolve and return the result
          if (promisesCompleted === taskList.length) {
            resolve(results);
          }
        })
        // if any promise fails, reject.
        .catch((error) => {
          reject(error);
        });
    });
  });
}
```

---

## Test Case 2: Rejection

```javascript
function task(time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      if (time < 3000) {
        reject("Rejected");
      } else {
        resolve(time);
      }
    }, time);
  });
}

const taskList = [task(1000), task(5000), task(3000)];

myPromiseAll(taskList)
  .then((results) => {
    console.log("got results", results);
  })
  .catch(console.error);

// Output: "Rejected"
```

---

> [!summary] Takeaway
> - `Promise.all()` resolves when **all** promises resolve, rejects on **first** rejection.
> - Results are returned in the **same order** as the input array.
> - Uses a **counter** to track completion and an **array** to store results by index.
> - The first rejection immediately rejects the entire promise.

---

### 📎 Reference

[Promise.all polyfill - LearnersBucket](https://learnersbucket.com/examples/interview/promise-all-polyfill/)

---
