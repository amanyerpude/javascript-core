
---

> [!quote] Metadata  
> **Posted on:** June 15, 2026  
> **Author:** Anuj Sharma  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises #polyfill

---

# Promise.race() Polyfill

Promise.race takes an iterable (such as array) of promises, and returns the first settled promise — here settled promise can be resolved or rejected. As name "race" suggests, whichever promise settled first will be returned by Promise.race() method.

---

## Example 1: First Settled Promise — Rejected

```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve("P1 resolved"), 100));
const p2 = new Promise((resolve) => setTimeout(() => resolve("P2 resolved"), 200));
const p3 = new Promise((resolve, reject) => setTimeout(() => reject("P3 rejected"), 50));

Promise.race([p1, p2, p3])
  .then((value) => console.log(`Fulfilled: ${value}`))
  .catch((error) => console.log(`Error: ${error}`));

// Output: "Error: P3 rejected"
```

## Example 2: First Settled Promise — Resolved

```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve("P1 resolved"), 100));
const p2 = new Promise((resolve) => setTimeout(() => resolve("P2 resolved"), 200));
const p3 = new Promise((resolve, reject) => setTimeout(() => reject("P3 rejected"), 150));

Promise.race([p1, p2, p3])
  .then((value) => console.log(`Fulfilled: ${value}`))
  .catch((error) => console.log(`Error: ${error}`));

// Output: "Fulfilled: P1 resolved"
```

---

## Expected Functionality

1. **First settled promise should be returned** (Resolved or Rejected)
2. **Non-promise values should resolve immediately**
3. **Immediate rejecting promise will be caught first**
4. **Throw error in case of invalid input** (not iterable)
5. **Empty array** — the returned promise will never settle

---

## Promise.race Polyfill Implementation

```javascript
function customRace(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Argument must be an iterable"));
    }

    for (const promise of promises) {
      Promise.resolve(promise).then(resolve, reject);
    }
  });
}
```

> [!tip] Key Points
> - Wrap each promise in `Promise.resolve()` to handle non-promise values.
> - The first promise to settle (resolve or reject) determines the outcome.
> - For empty arrays, the returned promise never settles.
> - For invalid input (non-iterable), throw a `TypeError`.

---

> [!summary] Takeaway
> - `Promise.race()` returns the **first settled** promise (resolved or rejected).
> - Non-promise values resolve **immediately**.
> - Empty arrays produce a promise that **never settles**.
> - Use `Promise.resolve()` wrapper to normalize all values to promises.

---

### 📎 Reference

[Promise.race Polyfill in Javascript - FrontendGeek](https://www.frontendgeek.com/blogs/promiserace-polyfill-in-javascript---detailed-explanation)

---
