
---

> [!quote] Metadata  
> **Posted on:** October 31, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #uber

---

# Implement mapLimit Async Function

Implement a `mapLimit` function that is similar to `Array.map()` which returns a promise that resolves on the list of output by mapping each input through an asynchronous iteratee function. It also accepts a limit to decide how many operations can occur at a time.

---

## Example

```javascript
let numPromise = mapLimit(
  [1, 2, 3, 4, 5],
  3,
  function (num, callback) {
    setTimeout(function () {
      num = num * 2;
      console.log(num);
      callback(null, num);
    }, 2000);
  }
);

numPromise
  .then((result) => console.log("success:" + result))
  .catch(() => console.log("no success"));

// Output:
// first batch: 2, 4, 6
// second batch: 8, 10
// "success: [2, 4, 6, 8, 10]"
```

---

## Implementation

```javascript
// helper function to chop array in chunks of given size
Array.prototype.chop = function (size) {
  const temp = [...this];
  if (!size) return temp;

  const output = [];
  let i = 0;
  while (i < temp.length) {
    output.push(temp.slice(i, i + size));
    i = i + size;
  }
  return output;
};

const mapLimit = (arr, limit, fn) => {
  return new Promise((resolve, reject) => {
    // chop the input array into subarrays of limit
    let chopped = arr.chop(limit);

    // run subarrays in series
    const final = chopped.reduce((a, b) => {
      return a.then((val) => {
        // run sub-array values in parallel
        return new Promise((resolve, reject) => {
          const results = [];
          let tasksCompleted = 0;

          b.forEach((e) => {
            fn(e, (error, value) => {
              if (error) {
                reject(error);
              } else {
                results.push(value);
                tasksCompleted++;
                if (tasksCompleted >= b.length) {
                  resolve([...val, ...results]);
                }
              }
            });
          });
        });
      });
    }, Promise.resolve([]));

    final
      .then((result) => resolve(result))
      .catch((e) => reject(e));
  });
};
```

---

## How It Works

1. **Chop** the input array into subarrays of the given limit size.
2. **Parent array** runs in series (next subarray after current finishes).
3. **Elements within** each sub-array run in parallel.
4. **Accumulate** results from each batch.
5. If any error occurs, **reject**.

---

> [!tip] Key Points
> - Combination of **Async.series** (batches) and **Async.parallel** (within batch).
> - Uses `reduce` to chain batches sequentially.
> - Each batch runs all elements concurrently with a Promise.
> - Callback-based iteratee function is wrapped in Promise.

---

> [!summary] Takeaway
> - `mapLimit` controls **concurrency** of async operations.
> - Chop array into chunks, run chunks in **series**, elements in **parallel**.
> - Returns a promise that resolves with all mapped results.
> - Useful for **rate-limiting** API calls or heavy operations.

---

### 📎 Reference

[Implement mapLimit async function - LearnersBucket](https://learnersbucket.com/examples/interview/implement-maplimit-async-function/)

---
