
---

> [!quote] Metadata  
> **Posted on:** September 12, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Promise.allSettled() Polyfill

Promise.allSettled takes an array of promises as input and returns an array with the result of all the promises whether they are rejected or resolved.

According to MDN:
> The Promise.allSettled() method returns a promise that fulfills after all of the given promises have either fulfilled or rejected, with an array of objects that each describes the outcome of each promise.

---

## Example

```javascript
const a = new Promise((resolve) => setTimeout(() => { resolve(3); }, 200));
const b = new Promise((resolve, reject) => reject(9));
const c = new Promise((resolve) => resolve(5));

allSettled([a, b, c]).then((val) => { console.log(val); });

// Output:
// [
//   { status: "fulfilled", value: 3 },
//   { status: "rejected", reason: 9 },
//   { status: "fulfilled", value: 5 }
// ]
```

---

## Implementation

```javascript
const allSettled = (promises) => {
  // map the promises to return custom response
  const mappedPromises = promises.map(
    (p) =>
      Promise.resolve(p).then(
        (val) => ({ status: 'fulfilled', value: val }),
        (err) => ({ status: 'rejected', reason: err })
      )
  );

  // run all the promises once with .all
  return Promise.all(mappedPromises);
};
```

---

## How It Works

1. **Map** each promise to return an object with `status` and `value`/`reason`.
2. Each promise is wrapped in `Promise.resolve()` to handle non-promise values.
3. Use `Promise.all()` to wait for all mapped promises to settle.
4. Returns an array of result objects regardless of resolve/reject.

---

> [!tip] Key Points
> - Unlike `Promise.all()`, `allSettled` **never rejects** — it always resolves.
> - Each result object has a `status` field: either `"fulfilled"` or `"rejected"`.
> - For fulfilled promises, the object has a `value` property.
> - For rejected promises, the object has a `reason` property.
> - Useful when you need the result of **every** promise, regardless of success/failure.

---

> [!summary] Takeaway
> - `Promise.allSettled()` **always resolves** with an array of status objects.
> - Each object contains `status`, `value` (fulfilled) or `reason` (rejected).
> - Never rejects — unlike `Promise.all()` which rejects on first failure.
> - Useful for **batch operations** where you want to know the outcome of each.

---

### 📎 Reference

[Promise.allSettled polyfill - LearnersBucket](https://learnersbucket.com/examples/interview/promise-allsettled-polyfill/)

---
