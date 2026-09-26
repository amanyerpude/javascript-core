
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** LearnersBucket  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #generator #promise

---

# Implement a Generator Function promiseAccumulation()

Implement a generator function `promiseAccumulation()` that receives an arbitrary number of promises and:
- If the promise is **resolved**, it yields the returned value.
- If the promise is **rejected**, it yields -1 and **stops** yielding values.

---

## Example

```javascript
const p1 = Promise.resolve(10);
const p2 = Promise.resolve(20);
const p3 = Promise.reject(-1);

const gen = promiseAccumulation(p1, p2, p3);

gen.next(); // { value: 10, done: false }
gen.next(); // { value: 20, done: false }
gen.next(); // { value: -1, done: true }
```

---

## Implementation

```javascript
async function* promiseAccumulation(...promises) {
  try {
    const results = await Promise.allSettled(promises);

    for (const result of results) {
      if (result.status === 'fulfilled') {
        yield result.value;
      } else {
        yield -1;
        return; // stop yielding
      }
    }
  } catch (error) {
    yield -1;
  }
}
```

---

## How It Works

1. Use `async function*` to create an **async generator**.
2. Accept arbitrary promises using **rest parameters**.
3. Use `Promise.allSettled()` to wait for all promises to settle.
4. Iterate through results:
   - **Fulfilled**: yield the value.
   - **Rejected**: yield -1 and **return** (stop the generator).
5. Wrap in `try/catch` for unexpected errors.

---

> [!tip] Key Points
> - `async function*` creates an **async generator function**.
> - `Promise.allSettled()` returns status objects for all promises.
> - `yield` pauses execution and returns the value.
> - `return` inside a generator **stops** iteration.
> - Use rest parameters `...promises` for arbitrary arguments.

---

> [!summary] Takeaway
> - Async generators combine **generators** and **promises**.
> - `Promise.allSettled()` handles both resolved and rejected promises.
> - `yield` returns values one at a time.
> - `return` stops the generator permanently.

---

### 📎 Reference

[Implement a generator function promiseAccumulation - StudyX](https://studyx.ai/homework/102107884-implement-a-generator-function-promise-accumulation-that-receives-an-arbitrary-number-of)

---
