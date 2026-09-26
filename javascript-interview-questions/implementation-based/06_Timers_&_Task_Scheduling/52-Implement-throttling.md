
---

> [!quote] Metadata  
> **Posted on:** January 1, 2020  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #throttling #performance

---

# Implement Throttling in JavaScript

Throttling is a technique for limiting the number of times a function can be called within a specified time. Unlike debouncing, throttled functions execute immediately when first called and ignore subsequent calls until a cooldown period has passed.

---

## Example

```javascript
let counter = 0;
const increment = () => {
  counter += 1;
  console.log(counter);
};

const throttledIncrement = throttle(increment, 500);

// First call executes immediately
throttledIncrement(); // logs 1

// These calls are ignored during the 500ms cooldown
throttledIncrement();
throttledIncrement();
throttledIncrement();

// After 500ms passes, the function can be called again
setTimeout(() => {
  throttledIncrement(); // logs 2
}, 600);
```

---

## Real-World Applications

Throttling is particularly useful when dealing with events that fire rapidly. Think about scroll events on a webpage — without throttling, a scroll handler might fire hundreds of times during a single scroll action. With throttling, you can limit it to executing maybe once every 100ms, which is more efficient.

---

## Building Our Own Throttle Function

### Step 1: Basic Function Signature

```javascript
function throttle(func, wait) {
  return function() {
  }
}
```

### Step 2: Track Cooldown Period

```javascript
function throttle(func, wait) {
  let timer;
  return function() {
  }
}
```

### Step 3: Throttling Logic

The throttling logic is straightforward:
- If the timer exists (we're in cooldown), ignore the call
- Otherwise, execute the function and start the cooldown timer

```javascript
function throttle(func, wait) {
  let timer;
  return function() {
    if (timer) return;
    func();
    timer = setTimeout(() => { timer = null }, wait);
  }
}
```

### Step 4: Handle `this` Context and Arguments

```javascript
function throttle(func, wait) {
  let timer;
  return function(...args) {
    if (timer) return;
    func.apply(this, args);
    timer = setTimeout(() => { timer = null }, wait);
  }
}
```

> [!tip] Key Points
> - We use a **regular function** instead of an arrow function for the returned function, so `this` refers to where the function is **called**, not where it's **defined**.
> - `func.apply(this, args)` ensures the original function gets both the correct context and all the arguments.
> - The `setTimeout` callback resets the timer to `null` after the wait period, allowing the function to be called again.

---

## Throttle vs Debounce

| Feature | Throttle | Debounce |
|---------|----------|----------|
| Execution | Executes immediately, then waits | Waits first, then executes |
| Use case | Need immediate response, limit subsequent calls | Wait until activity has completely stopped |

---

> [!summary] Takeaway
> - **Throttling** executes immediately and then waits for a cooldown period.
> - Uses **closures** and **setTimeout** to control execution rate.
> - Useful for **scroll, resize, mousemove** events that fire rapidly.
> - Different from **debouncing** which waits before executing.

---

### 📎 Reference

[JavaScript Throttling from Scratch - DEV Community](https://dev.to/eihab/javascript-throttling-from-scratch-11k2)

---
