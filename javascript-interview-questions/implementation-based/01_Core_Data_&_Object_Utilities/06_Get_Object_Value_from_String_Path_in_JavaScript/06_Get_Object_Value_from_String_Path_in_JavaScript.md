
---

> [!quote] Metadata  
> **Posted on:** June 4, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #object #path #lodash #polyfill #get

---

# Get Object Value from String Path in JavaScript

Implement a method in JavaScript that takes:

- an **object**, and
    
- a **string path** (or an array of strings)
    

and returns the value at that path.  
If nothing is found, return `undefined`.

This serves as a **polyfill for `lodash._get()`**.

---

## Example

**Input:**

```javascript
const obj = {
  a: {
    b: {
      c: [1,2,3]
    }
  }
};

console.log(get(obj, 'a.b.c')); 
console.log(get(obj, 'a.b.c.0')); 
console.log(get(obj, 'a.b.c[1]')); 
console.log(get(obj, 'a.b.c[3]'));
```

**Output:**

```javascript
// [1,2,3]
// 1
// 2
// undefined
```

---

## Concept

We want to safely access **deeply nested object properties** using a dynamic path.  
Steps to achieve this:

1. **Check path type**:
    
    - If it’s an array, join it into a string.
        
    - If it’s invalid or empty, return `undefined`.
        
2. **Normalize path**:
    
    - Remove special characters (`[`, `]`, `.`).
        
    - Extract only the relevant keys and indices.
        
3. **Traverse object**:
    
    - Use `Array.reduce()` to walk the path step by step.
        
    - If the chain breaks, return `undefined`.
        

---

## Implementation

```javascript
const get = (obj, path) => {
  // if path is not a string or array of string
  if (path === '' || path.length == 0) return undefined;
  
  // if path is an array, concatenate it and form a string
  if (Array.isArray(path)) path = path.join('.');
  
  // filter out brackets and dots
  let exactPath = [];
  for (let i = 0; i < path.length; i++) {
    if (path[i] !== '[' && path[i] !== ']' && path[i] !== '.') {
      exactPath.push(path[i]);
    }
  }
  
  // traverse the object
  const value = exactPath.reduce((source, path) => source[path], obj);
  
  // return value or undefined
  return value ? value : undefined;
};
```

---

## Extended Demo

**Input:**

```javascript
const obj = {
  a: {
    b: {
      c: [1,2,3]
    }
  }
};

console.log(get(obj, 'a.b.c'));        // [1,2,3]
console.log(get(obj, 'a.b.c.0'));      // 1
console.log(get(obj, 'a.b.c[1]'));     // 2
console.log(get(obj, ['a', 'b', 'c', '2'])); // 3
console.log(get(obj, 'a.b.c[3]'));     // undefined
console.log(get(obj, 'a.c'));          // undefined
```

**Output:**

```javascript
// [1,2,3]
// 1
// 2
// 3
// undefined
// undefined
```

---

> [!tip] Key Insights
> 
> - This mimics **Lodash’s `_get()`** behavior.
>     
> - Works for both **dot notation** and **bracket notation** paths.
>     
> - Safely handles **nested arrays** (`[index]`) and objects.
>     
> - Returns `undefined` if path is invalid, preventing runtime errors.
>     

---

> [!summary] Takeaway  
> By building a polyfill for `_.get()`, we learn about:
> 
> - **Dynamic property access**
>     
> - **String parsing & normalization**
>     
> - **Safe object traversal** with `reduce`
>     
> 
> This pattern is extremely useful for **config files, APIs, form data, and deeply nested JSON**.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/gXIHJCCDJPQ)

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/get-object-value-from-string-path/)

---
