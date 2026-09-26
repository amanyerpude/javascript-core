
---

> [!quote] Metadata  
> **Posted on:** January 14, 2023  
> **Author:** 
> **Posted in:** Interview, Javascript

---

# Currying – Part 4

Write a function that evaluates the following expression:

> [!example] Example
> 
> ```javascript
> function sum(a, b, c, d) {
>   return a + b + c + d;
> }
> 
> let curriedSum = curry(sum);
> 
> console.log(curriedSum(1,2,3,4,5));   // 10
> console.log(curriedSum(1)(2,3)(4,5)); // 10
> console.log(curriedSum(1)(2)(3)(4));  // 10
> ```

This is the fourth question in the series of currying where we use **Closure** to solve problems in JavaScript.

- [[Javascript function that returns sum of the previous values]]
    
- [[Currying in JavaScript]]
    
- [[Currying – Part 3]]
    

---

Currying in JavaScript is a concept of functional programming in which we can pass functions as arguments (callbacks) and return functions without any side effects (Changes to program states).

If you see the problem statement, we have a callback function `sum(a, b, c, d)` that accepts four arguments and returns their total.  
This callback function is passed to `curry`, which returns a new function.

When the number of arguments in this returned function becomes equal to the number of arguments the callback is expecting, the `curriedSum()` returns the total. Otherwise, it keeps returning another function that continues to accept arguments.

---

## Implementation

```javascript
let curry = (fn) => {
  
  // helper function
  let helper = (...args) => {
    
    // if we are receiving the expected number of arguments
    if (args.length >= fn.length) {
      // pass it to callback fn
      return fn(...args);
    } else {
      // return a new function that will accept the remaining arguments
      let temp = (...args2) => {
        
        // recursively call the same function
        // to validate if we have received the required arguments
        return helper(...args, ...args2);
      };
      
      // return the function
      return temp;
    }
  };
  
  // return helper
  return helper;
}
```

---

## Input / Output

```javascript
function sum(a, b, c, d) {
  return a + b + c + d;
}

let curriedSum = curry(sum);

console.log(curriedSum(1,2,3,4,5));   // 10
console.log(curriedSum(1)(2,3)(4));   // 10
console.log(curriedSum(1)(2)(3)(4));  // 10
```

**Output:**

```
10
10
10
```

---

> [!tip] Using `function.length`  
> In JavaScript, every function has a `.length` property that tells you the number of expected parameters.  
> Here, `fn.length` is used to decide when we’ve received enough arguments to evaluate the original callback function.

---

> [!summary] Takeaway  
> By leveraging **closures** and the **`.length` property of functions**, we can build a generic curry helper in JavaScript that:
> 
> - Accepts arguments flexibly (spread across multiple calls).
>     
> - Returns results once the required number of arguments is collected.
>     

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/gytPGYlK8gI)

---
