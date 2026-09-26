
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** Максим Синяков  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Promise.finally() Polyfill

Write your own `finally` implementation called `myFinally`.

The only argument is a callback:
- A function to asynchronously execute when this promise becomes settled.
- Its return value is ignored unless the returned value is a rejected promise.
- The function is called with no arguments.
- If the argument is omitted the method returns an equivalent promise.

---

## Example

```javascript
Promise.resolve(100)
  .myFinally(function foo(x) {
    throw "lol";
  })
  .then(
    x => console.log("then 1", x),
    x => console.log("then 2", x) // "lol"
  )
```

---

## Implementation

```javascript
Promise.prototype.myFinally = function (callback) {
  return this.then(
    (value) => Promise.resolve(callback()).then(() => value),
    (reason) => Promise.resolve(callback()).then(() => { throw reason; })
  );
};
```

---

## How It Works

1. `finally` receives a callback that runs when the promise settles (either resolved or rejected).
2. The callback's return value is **ignored** — the original value/reason is forwarded.
3. If the callback **throws** or returns a rejected promise, that error wins.
4. If the callback returns a **pending promise**, the chain waits for it.

---

> [!tip] Key Points
> - `finally` is syntactic sugar over `then` — it forwards the original value/reason.
> - The callback receives **no arguments**.
> - If the handler **throws**, the promise returned by `finally()` will be rejected with that value.
> - If the handler returns a **rejected promise**, that rejection replaces the original settlement.

---

> [!summary] Takeaway
> - `Promise.finally()` runs a callback when the promise **settles** (resolve or reject).
> - The original value/reason **passes through** unchanged.
> - If the callback **throws**, that error replaces the original result.
> - Can be implemented using `.then()` with two handlers.

---

### 📎 Reference

[.finally() polyfill implementation - Sinyakov](https://sinyakov.com/javascript/workbook/async/finally.html)

---
