
---

> [!quote] Metadata  
> **Posted on:** September 28, 2022  
> **Author:**  
> **Posted in:** Interview, JavaScript  
> **Tags:** #polyfill #groupBy #reduce #aggregation #interview

---

# `groupBy()` Polyfill in JavaScript

The **`groupBy()`** method groups elements of a collection based on a key. The key can be derived from either:

- a **function** applied to each element, or
    
- a **property name** of the element.
    

The task is to **write a polyfill** for `groupBy()` that accepts a collection and an iteratee, and returns an object with grouped values.

---

## Example

**Input:**

```javascript
groupBy([6.1, 4.2, 6.3], Math.floor);
groupBy(["one", "two", "three"], "length");
```

**Output:**

```javascript
// { 6: [6.1, 6.3], 4: [4.2] }
// { 3: ['one', 'two'], 5: ['three'] }
```

---

## Concept

- If the **iteratee is a function**, apply it to the element to compute the key.
    
- If the **iteratee is a property name**, use it to access the property of the element.
    
- Use **`Array.prototype.reduce()`** to aggregate values into groups efficiently.
    

This is a **classic interview question**, testing your ability to:

- Handle **dynamic iteratees** (functions vs. properties).
    
- Apply **reduce** to build objects.
    
- Think about **data transformation**.
    

---

## Implementation

```javascript
const groupBy = (values, keyFinder) => {
  // using reduce to aggregate values
  return values.reduce((a, b) => {
    // determine the key
    const key = typeof keyFinder === 'function' 
      ? keyFinder(b) 
      : b[keyFinder];
    
    // aggregate values based on the keys
    if (!a[key]) {
      a[key] = [b];
    } else {
      a[key] = [...a[key], b];
    }
    
    return a;
  }, {});
};
```

---

## Demo

**Input:**

```javascript
console.log(groupBy([6.1, 4.2, 6.3], Math.floor)); 
console.log(groupBy(["one", "two", "three"], "length")); 
```

**Output:**

```javascript
// { 6: [6.1, 6.3], 4: [4.2] }
// { 3: ['one', 'two'], 5: ['three'] }
```

---

## Alternative Version: Using `Object.groupBy` (ES2023+)

Modern JavaScript already provides **`Object.groupBy()`**:

```javascript
const result = Object.groupBy(["one", "two", "three"], (str) => str.length);
console.log(result);
// { 3: ['one', 'two'], 5: ['three'] }
```

But since this is **not supported everywhere**, building the polyfill prepares you for both **interviews** and **real-world fallback code**.

---

> [!tip] Key Insights
> 
> - **Dynamic iteratees** (functions or properties) make `groupBy` flexible.
>     
> - `reduce()` provides an elegant one-pass aggregation.
>     
> - This mirrors functionality from **Lodash** (`_.groupBy`).
>     
> - For large datasets, consider mutating arrays instead of spreading (`a[key].push(b)` instead of `[...a[key], b]`).
>     

---

> [!summary] Takeaway  
> Writing a `groupBy()` polyfill demonstrates:
> 
> - **Higher-order function usage** (function iteratee).
>     
> - **Object aggregation via reduce**.
>     
> - Understanding of **modern vs. legacy solutions**.
>     
> 
> A common **interview favorite** and a **real-world utility** for data transformation in JavaScript.

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/groupby-polyfill-in-javascript/)

---
