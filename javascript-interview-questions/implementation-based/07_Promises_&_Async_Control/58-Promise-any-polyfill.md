
---

> [!quote] Metadata  
> **Posted on:** November 27, 2022  
> **Author:** Awwfrontend  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Promise.any() Polyfill

Promise.any is a method that takes an array of promises and returns a promise which resolves with fulfilled values as soon as any of the input promises resolves, and rejects only if all the input promises reject.

---

## Example

```javascript
const p1 = new Promise((res, rej) => setTimeout(rej, 100, 'p1'));
const p2 = new Promise((res, rej) => setTimeout(rej, 200, 'p2'));

Promise.any([p1, p2])
  .then(console.log)
  .catch((err) => console.error(err.message));

// Output: "All promises were rejected"
```

When all promises reject, `Promise.any` throws an `AggregateError`.

---

## Implementation

```javascript
function myPromiseAny(promiseArray) {
  let errors = [];

  return new Promise((res, rej) => {
    promiseArray.forEach((promise, index) => {
      promise
        .then((data) => {
          res(data);
        })
        .catch((err) => {
          errors[index] = err;
          if (index === promiseArray.length - 1) {
            rej(new AggregateError(errors, 'All promises were rejected'));
          }
        });
    });
  });
}
```

---

## How It Works

1. Loop over the array of promises
2. If any promise resolves, immediately resolve the outer promise
3. If a promise rejects, store the error
4. If all promises have rejected (tracked by index), reject with `AggregateError`

---

> [!tip] Key Points
> - `Promise.any` resolves with the **first successful** promise.
> - Only rejects when **all** input promises reject.
> - Uses `AggregateError` to collect all rejection reasons.
> - Different from `Promise.race` which settles on the first settled promise (resolved or rejected).

---

> [!summary] Takeaway
> - `Promise.any()` resolves on the **first success**, rejects only when **all fail**.
> - Returns an `AggregateError` containing all rejection reasons.
> - Useful when you need **any one** of multiple async operations to succeed.

---

### 📎 Reference

[Implementing Promise.any polyfill - Medium](https://medium.com/@awwfrontend/implementing-promise-any-polyfill-promise-javascript-a915cceb9e0e)

---
