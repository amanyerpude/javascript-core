
---

> [!quote] Metadata  
> **Posted on:** April 2, 2024  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Execute Promises with Priority

Given a list of promises and their priorities, call them parallelly and resolve with the value of the first promise with the most priority. If all the promises fail then reject with a custom error.

---

## Example

```javascript
const promises = [
  { task: createAsyncTask(6), priority: 1 },
  { task: createAsyncTask(3), priority: 4 },
  { task: createAsyncTask(3), priority: 3 },
  { task: createAsyncTask(5), priority: 2 },
];

resolvePromisesWithPriority(promises).then((result) => {
  console.log(result);
}, (error) => {
  console.log(error);
});

// Output: 2
// (resolves with the priority value of the first resolved promise with highest priority)
```

---

## Implementation

```javascript
function resolvePromisesWithPriority(promises) {
  // sort the promises based on priority
  promises.sort((a, b) => a.priority - b.priority);

  // track the rejected promise
  let rejected = {};
  // track the result
  let result = {};
  // track the position of the most priority
  let mostPriorityIndex = 0;
  // track the no of promises executed
  let taskCompleted = 0;

  // return a new promise
  return new Promise((resolve, reject) => {
    // run each promise in parallel
    promises.forEach(({ task, priority }, i) => {
      task()
        .then((value) => {
          result[i] = value;
        })
        .catch((error) => {
          rejected[i] = true;
          // if the rejected promise is the least priority one
          // move to the next least priority
          if (i === mostPriorityIndex) {
            mostPriorityIndex++;
          }
        })
        .finally(() => {
          // if we have least priority promise resolved
          // then resolve with the priority value
          if (!rejected[mostPriorityIndex] && result[mostPriorityIndex]) {
            resolve(promises[mostPriorityIndex].priority);
          } else if (rejected[mostPriorityIndex]) {
            mostPriorityIndex++;
          }

          taskCompleted++;

          // if all the tasks are finished and none resolved
          if (taskCompleted === promises.length) {
            reject("All Apis Failed");
          }
        });
    });
  });
}
```

---

## Test Helper

```javascript
function createAsyncTask(val) {
  return function () {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (val > 5) {
          reject(val);
        } else {
          resolve(val);
        }
      }, val * 1000);
    });
  };
}
```

---

> [!tip] Key Points
> - Sort promises by priority in ascending order.
> - Execute all promises in parallel.
> - Track the most priority promise that resolves.
> - If a higher-priority promise rejects, move to the next.
> - If all reject, reject with a custom error.

---

> [!summary] Takeaway
> - Sort by priority, run in parallel, track results.
> - Resolve with the **highest priority** promise that succeeds.
> - Handle rejection by moving to the next priority level.
> - Reject with custom error if **all** promises fail.

---

### 📎 Reference

[Execute promises with the Priority - LearnersBucket](https://learnersbucket.com/examples/interview/execute-promises-with-the-priority/)

---
