
---

> [!info] Challenge Metadata  
> **Difficulty:** ⭐⭐ | **Category:** Array Operations | **Company:** Meta | **Frequency:** High  
> **Related:** [[02_Deep_Flatten_Object_in_JavaScript__01|Deep Flatten Object]], [[14_Polyfill_for_Array.map()_in_JavaScript|Array.map Polyfill]]  
> **Tags:** #array #flatten #flat #flatMap #polyfill #interview #meta

---

# How to Flatten an Array in JavaScript

Learn how to **flatten an array** in JavaScript.  
This question was once asked in **Meta’s frontend interview**.

Flattening an array means converting **nested sub-arrays (N-dimensional)** into a **single one-dimensional array**.

---

## Problem Statement

- Merge nested arrays into a single flat array.
    
- Support flattening to:
    
    - **One level**
        
    - **Specified depth**
        
    - **Infinite depth**
        
- Explore **native methods (`flat`, `flatMap`)** and **custom polyfills**.
    

---

## Example

Input:

```javascript
[[[1, [1.1]], 2, 3], [4, 5]]
```

Output:

```javascript
[1, 1.1, 2, 3, 4, 5]
```

---

## Native Solutions (ES2019+)

JavaScript ES2019 introduced two methods:

- **`Array.prototype.flat()`**
    
- **`Array.prototype.flatMap()`**
    

> ⚠️ Browser support:  
> Firefox 62+, Chrome 69+, Edge 76+, Safari 12+
> 
> If using **Babel**, code will be transpiled for older browsers.

---

### Using `flat()`

Flattens nested arrays into one array.

```javascript
[1, 2, [3, 4]].flat();
// [1, 2, 3, 4]
```

By default, it flattens **1 level**.  
You can pass a depth parameter:

```javascript
[1, 2, [3, 4]].flat(1);
// [1, 2, 3, 4]

[1, 2, [3, 4], [[5]]].flat(2);
// [1, 2, 3, 4, 5]
```

To flatten **any depth**, use `Infinity`:

```javascript
[[[1, [1.1]], 2, 3], [4, 5]].flat(Infinity);
// [1, 1.1, 2, 3, 4, 5]
```

---

### Using `flatMap()`

Combination of `map()` + `flat(1)`.  
Executes a mapping function and flattens the result.

```javascript
// map()
['Prashant Yadav', 'Learners Bucket'].map(e => e.split(' '));
// [["Prashant", "Yadav"], ["Learners", "Bucket"]]

// flatMap()
['Prashant Yadav', 'Learners Bucket'].flatMap(e => e.split(' '));
// ["Prashant", "Yadav", "Learners", "Bucket"]
```

---

## Polyfills (Custom Implementations)

In case you don’t want external libraries, you can build your own `flatten` function.

---

### Method 1: Recursive with `reduce()`

Uses modern ES6 features:

```javascript
const flatten = (arr) => {
  return arr.reduce((flat, toFlatten) => {
    return flat.concat(Array.isArray(toFlatten) ? flatten(toFlatten) : toFlatten);
  }, []);
};
```

---

### Method 2: Classic Recursive Loop

Without ES6, uses basic loops:

```javascript
const flatten = function(arr, result = []) {
  for (let i = 0, length = arr.length; i < length; i++) {
    const value = arr[i];
    if (Array.isArray(value)) {
      flatten(value, result);
    } else {
      result.push(value);
    }
  }
  return result;
};
```

---

> [!tip] Key Insights
> 
> - Use `flat()` for simple flattening — pass depth as needed.
>     
> - Use `flatMap()` when combining `map()` + flattening in one step.
>     
> - For **older browsers**, polyfills ensure compatibility.
>     
> - Recursive solutions help you understand **divide-and-conquer** and **array traversal**.
>     

---

> [!summary] Takeaway  
> Flattening arrays is a common interview problem and a practical skill.
> 
> - **Modern browsers:** Use `flat` or `flatMap`.
>     
> - **Compatibility required:** Use recursive **polyfills**.  
>     This problem reinforces knowledge of **array methods, recursion, and iteration strategies** in JavaScript.
>     

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/array/how-to-flat-an-array-in-javascript/)

---
