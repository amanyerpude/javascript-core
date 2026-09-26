
---

> [!quote] Metadata  
> **Posted on:** July 10, 2021  
> **Author:**   
> **Posted in:** Interview, JavaScript  
> **Tags:** #polyfill #map #array #prototype #interview

---

# Polyfill for `Array.map()` in JavaScript

JavaScript arrays have an inbuilt method **`map()`**, which takes a callback function as input and returns a **new array** containing the processed values returned by this function.

---

## Example

```javascript
const arr = [1, 2, 3];
const multipliedArr = arr.map((e) => e * 2);
console.log(multipliedArr);

// [2, 4, 6]
```

---

## Callback Arguments

The `map()` callback receives **three arguments**:

1. The **current element** being processed.
    
2. The **index** of the current element.
    
3. The **original array** being mapped.
    

This means we can access not only the value but also its position and the entire array while transforming it.

---

## Polyfill for `Array.map()`

This is an **extremely common JavaScript interview question**. You’re often asked to **implement a polyfill** for the `map()` method from scratch.

### What is a Polyfill?

A **polyfill** is a JavaScript fallback implementation that brings modern functionality (like `map()`) to older browsers that don’t support it natively.

---

### Requirements

Our `map()` polyfill should:

- Accept a **callback function** as an argument.
    
- Call the callback with `(element, index, array)` for every element.
    
- Return a **new array** with transformed values.
    

---

### Implementation

```javascript
Array.prototype.map = function (callback) {
  // Store the new array
  const result = [];
  
  for (let i = 0; i < this.length; i++) {
    // ensure the index actually exists in the array
    if (this.indexOf(this[i]) > -1) {
      result[i] = callback(this[i], i, this);
    }
  }
  
  // return the transformed array
  return result;
};
```

---

## Demo

```javascript
const arr = [1, 2, 3];

const doubled = arr.map((num, index, original) => {
  console.log(`Index: ${index}, Original: ${original}`);
  return num * 2;
});

console.log(doubled);
// [2, 4, 6]
```

---

## Improvements & Edge Cases

The above works but can be improved:

- **Handling sparse arrays:** `map()` should skip over empty slots (holes) in arrays.
    
- **Type checking:** Ensure the `callback` provided is actually a function.
    
- **Context binding:** `map()` also accepts an optional second argument (`thisArg`) to bind the context.
    

Here’s a more **robust version**:

```javascript
Array.prototype.map = function (callback, thisArg) {
  if (typeof callback !== "function") {
    throw new TypeError(callback + " is not a function");
  }

  const result = new Array(this.length);

  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      result[i] = callback.call(thisArg, this[i], i, this);
    }
  }

  return result;
};
```

---

> [!tip] Key Insights
> 
> - `map()` is **non-mutating**: it creates a **new array** instead of modifying the original.
>     
> - Works on **array-like objects** too, not just arrays.
>     
> - For sparse arrays, `map()` skips empty slots rather than treating them as `undefined`.
>     
> - Writing a polyfill forces you to understand **array traversal, callback invocation, and context binding**.
>     

---

> [!summary] Takeaway  
> Implementing a `map()` polyfill covers:
> 
> - **Closures & higher-order functions** (passing callbacks).
>     
> - **Prototype extension** (adding methods to `Array.prototype`).
>     
> - Handling **edge cases** (`thisArg`, sparse arrays).
>     
> 
> This is a **classic interview problem** and an essential step to mastering JavaScript internals.

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/polyfill-for-array-map/)

---
