
---

> [!quote] Metadata  
> **Posted on:** June 9, 2023  
> **Author:**  
> **Posted in:** Interview, JavaScript  
> **Tags:** #object #set #lodash #polyfill #recursion

---

# Set Object Value at the String Path in JavaScript

Given an **object**, a **path** (string or array of strings), and a **value**, update the object at that path.

This is a **polyfill for Lodash’s `_.set()` method**, and is essentially the **opposite of `_.get()`**.

---

## Example

```javascript
const object = { 'a': [{ 'b': { 'c': 3 } }] };

set(object, 'a[0].b.c', 4);
console.log(object.a[0].b.c);
// 4
 
set(object, ['x', '0', 'y', 'z'], 5);
console.log(object.x[0].y.z);
// 5
```

---

## Concept

Steps to implement:

1. **Path type check**
    
    - If `path` is a string → clean it (`[`, `]`, `.`) and split into keys.
        
    - If `path` is already an array → use it directly.
        
2. **Recursive helper**
    
    - Get the **current key** and the **remaining path**.
        
    - If there are remaining keys:
        
        - If the key doesn’t exist → create array/object depending on next key.
            
        - If the current value is not an object → replace it with array/object depending on next key.
            
        - Recurse deeper until the last key.
            
    - If no more keys left → assign the value directly.
        
3. **Return updated object**
    

⚠️ **Note**: This method **overrides existing values** at the path.

---

## Implementation

### Helper Function

```javascript
const helper = (obj, path, value) => {
  let [current, ...rest] = path;
  
  if (rest.length > 0) {
    if (!obj[current]) {
      const isNumber = `${+rest[0]}` === rest[0];
      obj[current] = isNumber ? [] : {};
    }
    
    if (typeof obj[current] !== 'object') {
      const isNumber = `${+rest[0]}` === rest[0];
      obj[current] = helper(isNumber ? [] : {}, rest, value);
    } else {
      obj[current] = helper(obj[current], rest, value);
    }
  } else {
    obj[current] = value;
  }
  
  return obj;
};
```

### Main `set` Function

```javascript
const set = (obj, path, value) => {
  let pathArr = path;
  
  if (typeof path === 'string') {
    pathArr = path.replace('[', '.').replace(']', '').split(".");
  }
  
  helper(obj, pathArr, value);
};
```

---

## Extended Demo

```javascript
const abc = {
  a: {
    b: { c: [1, 2, 3] },
    d: { a: "hello" }
  }
};

const instance1 = JSON.parse(JSON.stringify(abc));
set(instance1, 'a.b.c', 'learnersbucket');
console.log(instance1.a.b.c); // "learnersbucket"

const instance2 = JSON.parse(JSON.stringify(abc));
set(instance2, 'a.b.c.0', 'learnersbucket');
console.log(instance2.a.b.c[0]); // "learnersbucket"

const instance3 = JSON.parse(JSON.stringify(abc));
set(instance3, 'a.b.c[1]', 'learnersbucket');
console.log(instance3.a.b.c[1]); // "learnersbucket"

const instance4 = JSON.parse(JSON.stringify(abc));
set(instance4, ['a', 'b', 'c', '2'], 'learnersbucket');
console.log(instance4.a.b.c[2]); // "learnersbucket"

const instance5 = JSON.parse(JSON.stringify(abc));
set(instance5, 'a.b.c[3]', 'learnersbucket');
console.log(instance5.a.b.c[3]); // "learnersbucket"

const instance6 = JSON.parse(JSON.stringify(abc));
set(instance6, 'a.c.d[0]', 'learnersbucket');
console.log(instance6.a.c.d[0]); // "learnersbucket"

const instance7 = JSON.parse(JSON.stringify(abc));
set(instance7, 'a.d.01', 'learnersbucket');
console.log(instance7.a.d['01']); // "learnersbucket"

const object = { 'a': [{ 'b': { 'c': 3 } }] };
set(object, 'a[0].b.c', 4);
console.log(object.a[0].b.c); // 4

set(object, ['x', '0', 'y', 'z'], 5);
console.log(object.x[0].y.z); // 5
```

**Output:**

```javascript
"learnersbucket"
"learnersbucket"
"learnersbucket"
"learnersbucket"
"learnersbucket"
"learnersbucket"
"learnersbucket"
4
5
```

---

> [!tip] Key Insights
> 
> - Works with **dot notation** (`a.b.c`), **bracket notation** (`a[0].b`), and **array paths** (`['a','b','c']`).
>     
> - Automatically decides whether to create **arrays or objects** based on key type.
>     
> - Ensures paths are **recursively built** if they don’t exist.
>     
> - Overrides existing values at the target path.
>     

---

> [!summary] Takeaway  
> By creating a polyfill for Lodash’s `_.set()`, we learn how to:
> 
> - Handle **dynamic paths** for nested objects.
>     
> - Safely **mutate deeply nested values**.
>     
> - Build recursive utilities for **object manipulation**.
>     
> 
> This complements the `_.get()` polyfill and is a crucial tool for **config handling, API responses, and form state management**.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/NZ0JjE97P_U)

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/set-object-value-at-the-string-path/)

---
