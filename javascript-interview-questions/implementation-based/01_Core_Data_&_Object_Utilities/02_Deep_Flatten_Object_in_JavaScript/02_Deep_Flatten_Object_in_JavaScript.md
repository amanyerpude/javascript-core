
---

> [!quote] Metadata  
> **Posted on:** June 5, 2021  
> **Author:** 
> **Posted in:** Interview, JavaScript  
> **Tags:** #object #flatten #recursion #array #deep

---

# Deep Flatten Object in JavaScript – 1

Given a **nested object** (which may contain objects, arrays, or primitive values), the goal is to **deeply flatten it** into a new object where keys represent the full path.

---

## Problem Statement

- Input: A nested object with multiple levels (including arrays).
    
- Output: A flattened object where:
    
    - Nested object keys are concatenated using `"."`
        
    - Array indices are appended as part of the key
        

---

## Example

**Input:**

```javascript
{
  A: "12",
  B: 23,
  C: {
    P: 23,
    O: {
       L: 56
    },
    Q: [1, 2]
   }   
}
```

**Output:**

```javascript
{
  "A": "12",
  "B": 23,
  "C.O.L": 56,
  "C.P": 23,
  "C.Q.0": 1,
  "C.Q.1": 2
}
```

---

## Concept

In the output:

- Nested **objects** → keys are concatenated until reaching a primitive value.
    
- Nested **arrays** → indices are added to the flattened key.
    

There are two main approaches:

1. Using **recursion + type checking**
    
2. Converting **arrays to objects** before recursion (cleaner key handling).
    

We’ll explore both.

---

## Approach 1: Recursive Flatten (with Type Check)

Uses `Object.prototype.toString.call` to distinguish between objects and arrays.

```javascript
const flatten = (obj, prefix) => {
  // store the result
  let output = {};
  
  // iterate the object 
  for(let k in obj){
    let val = obj[k];
    
    // get the type
    const type = Object.prototype.toString.call(val);
    
    // object
    if(type === "[object Object]"){
      const newKey = prefix ? prefix + "." + k : k;
      const newObj = flatten(val, newKey);
      output = {...output, ...newObj};
    }
    // array
    else if(type === "[object Array]"){
      for(let i = 0; i < val.length; i++){
        const newKey = prefix ? prefix + "." + k + "." + i : k + "." + i;
        output = {...output, [newKey]: val[i]};
      }
    }
    // normal value
    else{
      const newKey = prefix ? prefix + "." + k : k;
      output = {...output, [newKey]: val};
    }
  }
  
  return output;
}
```

**Demo:**

```javascript
const nested = {
  A: "12",
  B: 23,
  C: {
    P: 23,
    O: { L: 56 },
    Q: [1, 2]
  }   
};

console.log(flatten(nested));
```

**Output:**

```javascript
{
  "A": "12",
  "B": 23,
  "C.O.L": 56,
  "C.P": 23,
  "C.Q.0": 1,
  "C.Q.1": 2
}
```

---

## Approach 2: Convert Arrays to Objects (Cleaner)

Instead of manually creating keys for arrays, convert them into objects and recurse.

```javascript
const flatten = (obj, prefix) => {
  let output = {};
  
  for(let k in obj){
    let val = obj[k];
    const newKey = prefix ? prefix + "." + k : k;
    
    if(typeof val === "object"){
      // handle array
      if(Array.isArray(val)){
        const { ...arrToObj } = val; // spread array into object
        const newObj = flatten(arrToObj, newKey);
        output = {...output, ...newObj};
      }
      // handle object
      else{
        const newObj = flatten(val, newKey);
        output = {...output, ...newObj};  
      }
    }
    // primitive value
    else{
      output = {...output, [newKey]: val};
    }
  }
  
  return output;
}
```

**Demo:**

```javascript
const nested = {
  A: "12",
  B: 23,
  C: {
    P: 23,
    O: { L: 56 },
    Q: [1, 2]
  }   
};

console.log(flatten(nested));
```

**Output:**

```javascript
{
  "A": "12",
  "B": 23,
  "C.O.L": 56,
  "C.P": 23,
  "C.Q.0": 1,
  "C.Q.1": 2
}
```

---

> [!tip] Key Insights
> 
> - Approach 1 works but creates **new keys repeatedly**, especially for arrays.
>     
> - Approach 2 is **cleaner** because arrays are converted into objects before recursion.
>     
> - Knowing both recursion styles helps in interviews and real-world JSON transformations.
>     

---

> [!summary] Takeaway  
> Deep flattening objects is useful for:
> 
> - **Config management** (flatten nested settings into dot-notation).
>     
> - **Form data processing**.
>     
> - **Database queries** (NoSQL often uses dot notation).
>     
> 
> The recursive approach improves understanding of **nested data structures, type checking, and object/array handling** in JavaScript.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/z3isDlyMqog)

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/deep-flatten-object-in-javascript-1/)

---
