
---

> [!quote] Metadata  
> **Posted on:** July 18, 2026  
> **Author:** InterviewsVector  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises #advanced

---

# Create Custom Promise

Implement a `MyPromise` class that passes for the real thing: `new MyPromise(executor)`, `then`/`catch`/`finally`, chaining, and correct async ordering.

---

## The Three Difficulty Cliffs

1. **The state machine** — pending → fulfilled/rejected, one-way, settle-once.
2. **Timing** — handlers must run as **microtasks**, never synchronously, even for already-settled promises.
3. **The resolution procedure** — when a handler returns a *promise*, the chain must wait for it and adopt its state.

---

## Implementation

```javascript
class MyPromise {
  #state = "pending";
  #result = undefined;
  #handlers = []; // queued { onFulfilled, onRejected, resolve, reject }

  constructor(executor) {
    try {
      executor(this.#resolve.bind(this), this.#reject.bind(this));
    } catch (err) {
      this.#reject(err);
    }
  }

  #resolve(value) {
    if (this.#state !== "pending") return; // settle-once
    if (value === this) {
      return this.#reject(new TypeError("Chaining cycle detected"));
    }

    // If value is a thenable, adopt its eventual state
    if (value !== null && (typeof value === "object" || typeof value === "function")) {
      let then;
      try {
        then = value.then;
      } catch (err) {
        return this.#reject(err);
      }
      if (typeof then === "function") {
        let called = false;
        try {
          then.call(
            value,
            (v) => { if (!called) { called = true; this.#resolve(v); } },
            (r) => { if (!called) { called = true; this.#reject(r); } }
          );
        } catch (err) {
          if (!called) { called = true; this.#reject(err); }
        }
        return;
      }
    }

    this.#state = "fulfilled";
    this.#result = value;
    this.#flush();
  }

  #reject(reason) {
    if (this.#state !== "pending") return;
    this.#state = "rejected";
    this.#result = reason;
    this.#flush();
  }

  #flush() {
    this.#handlers.forEach((h) => this.#schedule(h));
    this.#handlers = [];
  }

  #schedule({ onFulfilled, onRejected, resolve, reject }) {
    queueMicrotask(() => {
      try {
        if (this.#state === "fulfilled") {
          if (typeof onFulfilled === "function") {
            resolve(onFulfilled(this.#result));
          } else {
            resolve(this.#result); // passthrough
          }
        } else {
          if (typeof onRejected === "function") {
            resolve(onRejected(this.#result)); // handled rejection → recovers
          } else {
            reject(this.#result); // passthrough: rejection tunnels to next catch
          }
        }
      } catch (err) {
        reject(err);
      }
    });
  }

  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      const handler = { onFulfilled, onRejected, resolve, reject };
      if (this.#state === "pending") {
        this.#handlers.push(handler);
      } else {
        this.#schedule(handler); // already settled → still a microtask
      }
    });
  }

  catch(onRejected) {
    return this.then(undefined, onRejected);
  }

  finally(onFinally) {
    return this.then(
      (value) => MyPromise.resolve(onFinally()).then(() => value),
      (reason) => MyPromise.resolve(onFinally()).then(() => { throw reason; })
    );
  }

  static resolve(value) {
    return value instanceof MyPromise
      ? value
      : new MyPromise((resolve) => resolve(value));
  }

  static reject(reason) {
    return new MyPromise((_, reject) => reject(reason));
  }
}
```

---

## Verified Behavior

```javascript
// 1. Microtask timing
const p = MyPromise.resolve("A");
p.then(console.log);
console.log("B");
// B, then A — handlers NEVER run synchronously

// 2. catch on a FULFILLED promise (passthrough)
MyPromise.resolve("ok")
  .then((v) => v + "!")
  .catch(() => "never runs")
  .then(console.log); // "ok!"

// 3. Returning a promise from then (resolution procedure)
MyPromise.resolve(1)
  .then((v) => new MyPromise((res) => setTimeout(() => res(v + 1), 10)))
  .then(console.log); // 2 — chain WAITED for inner promise

// 4. Error recovery
MyPromise.reject(new Error("boom"))
  .catch((e) => "recovered")
  .then(console.log); // "recovered"
```

---

> [!summary] Takeaway
> - **State machine**: pending → fulfilled/rejected, settle-once.
> - **Microtask timing**: handlers always run as microtasks, never synchronously.
> - **Resolution procedure**: adopt thenable state for chaining.
> - **Handler passthrough**: `then(null).then(v => ...)` forwards values correctly.
> - **`called` flag**: protects against hostile thenables calling back twice.

---

### 📎 Reference

[Build a Promise from Scratch - InterviewsVector](https://www.interviewsvector.com/javascript/custom-promises)

---
