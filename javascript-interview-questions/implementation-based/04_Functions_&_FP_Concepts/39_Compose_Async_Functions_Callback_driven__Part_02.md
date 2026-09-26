

---

> [!quote] Metadata  
> **Asked in:** Adobe MTS2 Front-end Engineer Interview  
> **Posted in:** Interview, Javascript  
> **Tags:** #function #compose #callback

---

# Compose Async Functions – Part 2 (Callback-driven)

This is a variation of [[Create Compose Async Functions with Chaining Support]] where functions are **synchronous in nature but control execution via an explicit `next` callback**.

---

## Problem Statement

- Create a **`compose`** function that accepts multiple functions (`C, B, A`) in reverse order.
    
- Each function receives its arguments **plus a `next` callback**.
    
- A function must call `next(error, result)` to proceed.
    
- The `compose` function should return another function that accepts:
    
    - Initial inputs
        
    - A final callback (`done`) which receives `(error, result)`.
        

---

> [!example] Example Functions
> 
> ```javascript
> // A: multiply x and y
> const A = (x, y, next) => {
>   setTimeout(() => next(null, x * y), 100);
> };
> 
> // B: add 5
> const B = (val, next) => {
>   setTimeout(() => next(null, val + 5), 100);
> };
> 
> // C: divide by 10
> const C = (val, next) => {
>   setTimeout(() => next(null, val / 10), 100);
> };
> 
> // Compose chain
> const done = (err, result) => {
>   if (err) return console.error("Error:", err);
>   console.log("Final Result:", result);
> };
> 
> compose(C, B, A)(5, 3, done); // Expected output: 2
> ```

---

## Concept

- **Execution order:** Compose → right-to-left (`A → B → C`).
    
- **Callback-driven:** Each function controls when the next executes.
    
- **Closures:** The internal `next` helper remembers the `index`, `functions`, and `callback`.
    
- **Termination conditions:**
    
    - If an error is passed → immediately invoke `done(err)`.
        
    - If no functions remain → invoke `done(null, result)`.
        

---

## Implementation

```javascript
const compose = (...fns) => {
  return (...args) => {
    const callback = args.pop(); // final done callback
    let index = fns.length - 1;

    function next(err, result) {
      // error short-circuit
      if (err) return callback(err);

      // no more functions, return result
      if (index === 0) return callback(null, result);

      // move to previous function
      index--;
      fns[index](result, next);
    }

    // initial kick-off
    fns[index](...args, next);
  };
};
```

---

## Input / Output

```javascript
const A = (x, y, next) => {
  setTimeout(() => next(null, x * y), 1000);
};

const B = (val, next) => {
  setTimeout(() => next(null, val + 5), 1000);
};

const C = (val, next) => {
  setTimeout(() => next(null, val / 10), 1000);
};

const done = (err, result) => {
  if (err) return console.error("Error:", err);
  console.log("Final Result:", result);
};

compose(C, B, A)(5, 3, done);
```

**Execution Trace:**

1. `A(5,3)` → `15`
    
2. `B(15)` → `20`
    
3. `C(20)` → `2`
    
4. `done(null, 2)`
    

**Output:**

```javascript
Final Result: 2
```

**Note:** With `setTimeout` at `1000ms` each, total delay ≈ 3 seconds.

---

> [!tip] Key Insights
> 
> - Unlike Part 1 (`composeAsync` with Promises), here control is **explicitly managed via `next` callbacks**.
>     
> - Demonstrates understanding of **closures**, **execution flow**, and **callback-based async control**.
>     
> - Common pattern in frameworks like **Express.js middleware**.
>     

---

> [!summary] Takeaway  
> This variation of `compose` tests callback-based control of async flow, error handling, and closure usage — a classic higher-level interview problem.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/zVvIn9aZLos?si=7YmV9kHxrdziz-F5)

---
