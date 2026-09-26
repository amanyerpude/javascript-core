
---

> [!quote] Metadata  
> **Posted on:** November 9, 2019  
> **Author:**   
> **Posted in:** Arrays, Interview, JavaScript  
> **Tags:** #compare #deepEqual #array #object #equality #interview

---

# Compare Two Arrays or Objects with JavaScript

Learn how to **compare array or object** with JavaScript.

There are no specific built-in methods to deeply compare two arrays or objects in JavaScript.  
So we’ll build our **own custom functions**—from a quick-and-dirty approach to a **robust deep comparator** used in interviews and real projects.

---

## Problem Statement

- Compare two inputs that could be **arrays or objects** (including nested/complex structures).
    
- Start with a **basic approach** (stringifying) and understand where it fails.
    
- Build a **reliable comparison** that:
    
    - Confirms **same type** (array vs object)
        
    - Checks **same size/shape**
        
    - **Recursively** compares nested values
        
    - Handles **functions** (by string representation)
        

---

## Basic Approach: Convert to String

The most basic approach is to convert the whole array or object to a string and compare:

```javascript
let a = [1, 2, 3, 4, 5];
let b = [1, 2, 3, 4, 5];

// "[1, 2, 3, 4, 5]"==="[1, 2, 3, 4, 5]"
console.log(JSON.stringify(a) === JSON.stringify(b));

// true
```

This **seems** fine—but it **fails** when the element order differs (or for objects with different key order):

```javascript
let a = [1, 2, 3, 4, 5];
let b = [1, 2, 4, 5, 3];

// "[1, 2, 3, 4, 5]"==="[1, 2, 4, 5, 3]"
console.log(JSON.stringify(a) === JSON.stringify(b));

// false
```

Technically, both arrays contain the same elements and are equal in length, but string comparison is **order-sensitive**. Same with objects:

```javascript
let a = {a: 1, b: 2, c: 3};
let b = {b: 2, a: 1, c: 3};

//"{'a':1,'b':2,'c':3}" "{'b':2,'a':1,'c':3}"
console.log(JSON.stringify(a) === JSON.stringify(b));

// false
```

---

## Compare **Only Arrays** (Single & Multi-Dimensional)

Create a comparator for arrays (and nested arrays). Logic:

1. If either input is falsy → `false`.
    
2. If lengths differ → `false`.
    
3. Iterate:
    
    - If both elements are arrays → **recurse**
        
    - Else compare with strict equality
        
    - Short-circuit on first mismatch
        

```javascript
let compare = (arr1,arr2) => {
  // If not array then return false
  if(!arr1 || !arr2) return false;
  
  if(arr1.length !== arr2.length) return false;

  let result;

  for(let i = 0; i < arr1.length; i++){
    if(Array.isArray(arr1[i]) && Array.isArray(arr2[i])){
      result = compare(arr1[i], arr2[i]);
    } else if(arr1[i] === arr2[i]){
      result = true;
    } else {
      result = false;
    }
    if(!result){
      break;
    }
  }

  return result;
}

console.log(compare([1,2,3],[1,2,3]));                         // true
console.log(compare([1,2],[1,2,3]));                           // false
console.log(compare([[1, 2], [3, 4]],[[1, 2],[3, 4, 6]]));     // false
console.log(compare([[1, 2], [3, 4]],[[1, 2], [3, 4]]));       // true
console.log(compare([1, 2, [3, 4, 5]],[1, 2, [3, 4, 5]]));     // true
console.log(compare([], [1, 2, [3, 4, 5]]));                   // false
```

> This method is great for **arrays of primitives/arrays**.  
> It **fails** if elements can be **objects or functions**.

---

## Method 3: Compare **Array or Object** (Robust Deep Equality)

A more robust deep comparison that handles:

- Arrays and Objects (possibly nested)
    
- Functions (by **string value**)
    
- Strict equality for primitives
    

### What to Check

- They are **array/object** types (not other types).
    
- They are of the **same type** (both arrays or both objects).
    
- They have the **same number of elements/keys**.
    
- Each pair of corresponding elements/values:
    
    - Are of the **same type**
        
    - Are **strictly equal**, or (if array/object) **recursively equal**
        
    - If they are **functions**, compare **`toString()`** output
        

### Skeleton

```javascript
let compare = (current, other) => {
  // Comparison will be done here.
}
```

### Step 1 — Basic Type Tests

```javascript
let compare = (current, other) => {
  // Get the inputs type
  let currentType = Object.prototype.toString.call(current);
  let otherType = Object.prototype.toString.call(other);

  // If items are not an object or array, return false
  if (['[object Array]', '[object Object]'].indexOf(currentType) < 0 ||
      ['[object Array]', '[object Object]'].indexOf(otherType) < 0) return false;

  // If the two inputs are not the same type, return false
  if (currentType !== otherType) return false;

  // Other comparisons will happen here

  // If all tests are passed then
  return true;
}
```

### Step 2 — Same Size?

```javascript
let compare = (current, other) => {
  let currentType = Object.prototype.toString.call(current);
  let otherType = Object.prototype.toString.call(other);

  if (['[object Array]', '[object Object]'].indexOf(currentType) < 0 ||
      ['[object Array]', '[object Object]'].indexOf(otherType) < 0) return false;

  if (currentType !== otherType) return false;

  // Compare lengths
  let currentLen = currentType === '[object Array]' ? current.length : Object.keys(current).length;
  let otherLen   = otherType   === '[object Array]' ? other.length   : Object.keys(other).length;
  if (currentLen !== otherLen) return false;

  // Other comparisons will happen here
  return true;
}
```

### Step 3 — Compare Elements/Properties

Prepare iteration and delegate value comparison to a helper:

```javascript
let compare = (current, other) => {
  let currentType = Object.prototype.toString.call(current);
  let otherType = Object.prototype.toString.call(other);

  if (['[object Array]', '[object Object]'].indexOf(currentType) < 0 ||
      ['[object Array]', '[object Object]'].indexOf(otherType) < 0) return false;

  if (currentType !== otherType) return false;

  let currentLen = currentType === '[object Array]' ? current.length : Object.keys(current).length;
  let otherLen   = otherType   === '[object Array]' ? other.length   : Object.keys(other).length;
  if (currentLen !== otherLen) return false;

  // Compare properties
  if (currentType === '[object Array]') {
    for (var i = 0; i < currentLen; i++) {
      // Compare the item
      // (helper will be declared below)
    }
  } else {
    for (var key in current) {
      if (current.hasOwnProperty(key)) {
        // Compare the item
      }
    }
  }

  return true;
}
```

### Helper: `equal(item1, item2)`

- If value is array/object → **recurse** with `compare`
    
- Else:
    
    - Types must match
        
    - If **function** → compare **`toString()`**
        
    - Else → **strict equality**
        

```javascript
let equal = (item1, item2) => {
  // Get the object type
  let itemType = Object.prototype.toString.call(item1);

  // If an object or array, compare recursively
  if (['[object Array]', '[object Object]'].indexOf(itemType) >= 0) {
    if (!compare(item1, item2)) return false;
  } else {
    // Types must match
    if (itemType !== Object.prototype.toString.call(item2)) return false;

    // Function: compare by string
    if (itemType === '[object Function]') {
      if (item1.toString() !== item2.toString()) return false;
    } else {
      if (item1 !== item2) return false;
    }
  }
};
```

### Full Implementation

Plug `equal()` into `compare()` and short-circuit on any mismatch:

```javascript
let compare = (current, other) => {
  // Get the inputs  type
  let currentType = Object.prototype.toString.call(current);
  let otherType = Object.prototype.toString.call(other);

  // If items are not an object or array, return false
  if (['[object Array]', '[object Object]'].indexOf(currentType) < 0 ||
      ['[object Array]', '[object Object]'].indexOf(otherType) < 0) return false;

  // If the two inputs are not the same type, return false
  if (currentType !== otherType) return false;

  // Compare the length of the two items
  let currentLen = currentType === '[object Array]' ? current.length : Object.keys(current).length;
  let otherLen   = otherType   === '[object Array]' ? other.length   : Object.keys(other).length;
  if (currentLen !== otherLen) return false;

  // Helper to compare individual values
  let equal = (item1, item2) => {
    let itemType = Object.prototype.toString.call(item1);

    if (['[object Array]', '[object Object]'].indexOf(itemType) >= 0) {
      if (!compare(item1, item2)) return false;
    } else {
      if (itemType !== Object.prototype.toString.call(item2)) return false;

      if (itemType === '[object Function]') {
        if (item1.toString() !== item2.toString()) return false;
      } else {
        if (item1 !== item2) return false;
      }
    }
  };

  // Compare properties/elements
  if (currentType === '[object Array]') {
    for (var i = 0; i < currentLen; i++) {
      if (equal(current[i], other[i]) === false) return false;
    }
  } else {
    for (var key in current) {
      if (current.hasOwnProperty(key)) {
        if (equal(current[key], other[key]) === false) return false;
      }
    }
  }

  // If all tests pass
  return true;
}
```

### Demos

```javascript
let arr1 = [1, 2, 3, 4, 5];
let arr2 = [1, 3, 2, 4, 5];
console.log(compare(arr1, arr2)); 
// returns false

let arrObj1 = [1, 2, {
  a: 1,
  b: 2,
  c: 3,
  d: function(){
    console.log("abcd");
  }
}, 4, 5];
let arrObj2 = [1, 2, {
  c: 3,
  b: 2,
  a: 1,
  d: function(){
    console.log("abcd");
  }
}, 4, 5];
console.log(compare(arrObj1, arrObj2)); 
// returns true

let arr4 = [[1, 2], [3, 4, 5]];
let arr3 = [[1, 2], [3, 4, 5]];
console.log(compare(arr4, arr3)); 
// returns true
```

If you want, you can use libraries as well, e.g. **Lodash**: `_.isEqual(obj1, obj2)`.

---

> [!tip] Key Insights
> 
> - **`JSON.stringify`** is fast but **order-sensitive**; good for quick checks, not deep equality.
>     
> - A proper deep comparator must confirm **type**, **size**, and **recursive equality**.
>     
> - Comparing **functions** via `toString()` works for many cases, but note it can fail with **closures** or **environment differences**.
>     
> - Real-world extras you may want to handle: `Date` (compare `getTime()`), `RegExp` (source+flags), `NaN` (treat `NaN === NaN` as true), `-0/+0`, and structures like `Map`/`Set` (iterate entries).
>     

---

> [!summary] Takeaway  
> For interviews and production code, a **robust deep equality** function is invaluable.
> 
> - Use **stringify** only when order and formatting are guaranteed.
>     
> - For nested arrays/objects (with functions), prefer a **recursive comparator**.
>     
> - Libraries like **Lodash `_.isEqual`** save time, but understanding the **mechanics** makes you a stronger engineer.
>     

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/array/compare-two-array-or-object-with-javascript/)

---
