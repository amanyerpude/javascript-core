
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** FrontPrep  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async

---

# Implement Async Reject

Implement an `asyncReject` function that works like `Array.filter()` but with an async predicate function. It filters out elements for which the async predicate resolves to `true`.

---

## Example

```javascript
const isEven = async (num) => {
  return new Promise((resolve) => {
    setTimeout(() => resolve(num % 2 === 0), 100);
  });
};

asyncReject([1, 2, 3, 4, 5], isEven)
  .then(console.log);

// Output: [2, 4]
```

---

## Implementation

```javascript
const asyncReject = async (arr, predicate) => {
  const results = [];

  for (const item of arr) {
    const shouldReject = await predicate(item);
    if (shouldReject) {
      results.push(item);
    }
  }

  return results;
};
```

Or using `Promise.all` for parallel execution:

```javascript
const asyncReject = async (arr, predicate) => {
  const results = await Promise.all(
    arr.map(async (item) => {
      const shouldReject = await predicate(item);
      return shouldReject ? item : null;
    })
  );

  return results.filter((item) => item !== null);
};
```

---

> [!tip] Key Points
> - Similar to `Array.filter()` but supports **async** predicate functions.
> - Use `for...of` with `await` for **sequential** execution.
> - Use `Promise.all` with `.map()` for **parallel** execution.
> - Choose sequential vs parallel based on the use case.

---

> [!summary] Takeaway
> - `asyncReject` filters array elements using an **async** predicate.
> - Sequential approach: iterate with `for...of` and `await`.
> - Parallel approach: use `Promise.all` with `.map()`.
> - Opposite of `asyncFilter` — keeps elements where predicate is `true`.

---

### 📎 Reference

[async reject - FrontPrep](https://www.frontprep.com/javascript-coding/async-reject)

---
