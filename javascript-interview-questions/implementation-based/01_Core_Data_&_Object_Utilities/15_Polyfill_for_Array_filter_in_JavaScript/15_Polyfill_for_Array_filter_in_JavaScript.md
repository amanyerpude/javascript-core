
---

> [!quote] Metadata  
> **Posted on:** August 3, 2021  
> **Author:** 
> **Posted in:** Interview, JavaScript  
> **Tags:** #polyfill #filter #array #prototype #interview

---

# Polyfill for `Array.filter()` in JavaScript

JavaScript arrays have an inbuilt method **`filter()`**, which takes a **callback function** as input and returns a **new array** containing all the elements that pass the test implemented by the callback function.

---

## Example

```javascript
const arr = [1, 2, 3, 4, 5, 6];
const filtered = arr.filter((e) => e % 2 === 0);
console.log(filtered);

// [2, 4, 6]
```

---

## Callback Arguments

The `filter()` callback receives **three arguments**:

1. The **current element** being processed.
    
2. The **index** of the element.
    
3. The **original array**.
    

This allows filtering logic to be **customized** based on element value, position, or even the entire array state.

---

## Polyfill for `Array.filter()`

This is a **classic JavaScript interview question**, where you’re often asked to implement the polyfill for `filter()` from scratch.

### What is a Polyfill?

A **polyfill** is a fallback (in plain JavaScript) that brings modern functionality (like `filter()`) to **older browsers** that don’t support it natively.

---

### Requirements

Our polyfill for `filter()` should:

- Accept a **callback function** as an argument.
    
- Call the callback with `(element, index, array)` for each element.
    
- Return a **new array** with elements that return truthy values.
    

---

### Basic Implementation

```javascript
Array.prototype.filter = function (callback) {
  // Store the new array
  const result = [];
  
  for (let i = 0; i < this.length; i++) {
    // call the callback with element, index, and array
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  
  // return the new array
  return result;
};
```

---

## Demo

```javascript
const arr = [1, 2, 3, 4, 5, 6];

const evenNumbers = arr.filter((num, index, array) => {
  console.log(`Checking index ${index}, value ${num}`);
  return num % 2 === 0;
});

console.log(evenNumbers);
// [2, 4, 6]
```

---

## Improvements & Edge Cases

The above implementation works but can be **enhanced**:

- **Type checking** → ensure the `callback` is actually a function.
    
- **Handling sparse arrays** → native `filter()` skips empty slots (holes).
    
- **Context binding (`thisArg`)** → `filter()` allows an optional second argument for callback context.
    

Here’s a more **robust polyfill**:

```javascript
Array.prototype.filter = function (callback, thisArg) {
  if (typeof callback !== "function") {
    throw new TypeError(callback + " is not a function");
  }

  const result = [];

  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      if (callback.call(thisArg, this[i], i, this)) {
        result.push(this[i]);
      }
    }
  }

  return result;
};
```

---

## Native vs Polyfill

- Native `filter()` automatically **skips empty slots** in sparse arrays:
    

```javascript
const arr = [1, , 3]; // hole at index 1
console.log(arr.filter(Boolean)); 
// [1, 3]
```

- Our robust polyfill above handles this by checking `if (i in this)`.
    

---

> [!tip] Key Insights
> 
> - `filter()` always returns a **new array**, leaving the original untouched.
>     
> - Works on **array-like objects** too.
>     
> - Can bind a **custom context** (`thisArg`).
>     
> - Writing a polyfill forces you to understand **loops, callbacks, and array holes**.
>     

---

> [!summary] Takeaway  
> Implementing a `filter()` polyfill ensures you understand:
> 
> - **Callback mechanics** `(element, index, array)`.
>     
> - **Prototype extension** for polyfilling.
>     
> - Handling **edge cases** like sparse arrays and custom `this` binding.
>     
> 
> This is one of the **most common JavaScript interview questions**, bridging practical coding and deep API understanding.

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/polyfill-for-array-filter/)

---
