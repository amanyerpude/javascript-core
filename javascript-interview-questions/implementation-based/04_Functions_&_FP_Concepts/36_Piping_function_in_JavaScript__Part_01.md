
---

> [!quote] Metadata  
> **Posted on:** February 1, 2023  
> **Author:** 
> **Posted in:** Interview, Javascript

---

# Piping function in JavaScript – Part 1

Given an object which can have a function as a value at a nested level, create a function that will accept arguments as input and pass them through all the functions in the object, returning the computed value.

---

> [!example] Example  
> **Input:**
> 
> ```javascript
> {
>   a : {
>     b : (a,b,c) => a+b+c,
>     c : (a,b,c) => a+b-c,
>   },
>   d : (a,b,c) => a-b-c
> }
> 
> Fn(obj)(1,1,1);
> ```
> 
> **Output:**
> 
> ```javascript
> {
>   a : {
>     b : 3,
>     c : 1
>   },
>   d: -1
> }
> ```

---

## Concept

We can solve this by forming a **closure**:

1. Create a function that accepts the object as input.
    
2. Return another function that accepts the arguments.
    
3. Inside the inner function, iterate over all keys:
    
    - If the value is a function → call it with the arguments, replace the value with the result.
        
    - Else, recursively call the same function for nested objects.
        
4. Return the modified object (processed **in place**).
    

---

## Implementation

```javascript
const pipe = (obj) => {
  // return another function that will accept all the args
  return function(...args) {
    // iterate the keys of the object
    for (let key in obj) {
      let val = obj[key];
      
      // if the value is a function
      if (typeof val === 'function') {
        obj[key] = val(...args); // replace with computed result
      }
      else {
        // recursively process nested objects
        obj[key] = pipe(val)(...args);
      }
    }
    
    // return the object after processing
    return obj;
  }
};
```

---

## Input / Output

```javascript
let test = {
  a: {
    b: (a, b, c) => a + b + c,
    c: (a, b, c) => a + b - c,
  },
  d: (a, b, c) => a - b - c,
  e: 1,
  f: true
};

console.log(pipe(test)(1, 1, 1));
```

**Output:**

```javascript
{
  "a": {
    "b": 3,
    "c": 1
  },
  "d": -1,
  "e": 1,
  "f": true
}
```

---

> [!tip] Key Insight
> 
> - This works because of **closures** (to hold the reference to the input object) and **recursion** (to process nested objects).
>     
> - Non-function values remain unchanged in the output.
>     

---

> [!summary] Takeaway  
> A **piping function** can recursively traverse an object, apply arguments to all nested functions, and return the computed object — enabling flexible function evaluation across deeply nested structures.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/anvBprk5q-c)

---
