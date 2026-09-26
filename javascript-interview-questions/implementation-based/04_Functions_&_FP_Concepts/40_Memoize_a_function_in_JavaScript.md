
---

> [!quote] Metadata  
> **Posted on:** February 3, 2022  
> **Author:** 
> **Posted in:** Interview, Javascript  
> **Tags:** #function

---

# Memoize a function in JavaScript

Create a function in JavaScript that **memoizes or caches the result** for the given input so that the subsequent calls for the same inputs will be faster.

---

> [!example] Example
> 
> ```javascript
> slowFunc(params) // normal call, slow output
> 
> const memoized = memoize(slowFunc);
> memoized(params) // first call, extremely slow -> caches the result
> memoized(params) // second call, very fast
> 
> // all subsequent calls with the same input will be faster
> 
> memoized(differentParams) // first call, slow -> caches the result
> memoized(differentParams) // second call, very fast
> ```

---

## Concept

Memoization is an **optimization technique** used in computer science to avoid recomputation in intensive computation:

- Cache the result for a given input.
    
- On the next call with the same input, return the cached result.
    
- Only compute again if the input is new.
    

We achieve this using a **closure with a higher-order function** that stores results in a cache object.

---

## Implementation

```javascript
const memoize = function(fn) {
  const cache = {};
  return function() {
    // use arguments as the key
    const KEY = JSON.stringify(arguments);
    
    // return cached result if present
    if (cache[KEY]) {
      return cache[KEY];
    }
    
    // else compute, cache, and return
    const evaluatedValue = fn(...arguments);
    cache[KEY] = evaluatedValue;
    return evaluatedValue;
  }
};
```

---

## Input / Output

```javascript
function factorial(n) {
   if (n === 0 || n === 1) {
     return 1;
   }
   return factorial(n-1) * n; 
}

const memoizedFactorial = memoize(factorial);

let a = memoizedFactorial(100); // slow
console.log(a);

let b = memoizedFactorial(100); // faster
console.log(b);
```

**Output:**

```javascript
9.33262154439441e+157 // slow
9.33262154439441e+157 // faster
```

---

> [!tip] Key Insight
> 
> - Memoization = **store results** of expensive function calls.
>     
> - Future calls with the same arguments return instantly.
>     
> - Achieved using **closures** + **cache object** keyed by arguments.
>     

---

> [!summary] Takeaway  
> Memoization improves performance by caching expensive function results and returning them instantly on repeated calls with the same inputs.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/ktL5L2DnYYg)

---
