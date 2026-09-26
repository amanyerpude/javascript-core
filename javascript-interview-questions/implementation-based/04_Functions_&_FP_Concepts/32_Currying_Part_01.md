
---

> [!quote] Metadata  
> **Posted on:** January 4, 2020  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #Interview #Javascript #Easy

---

# Javascript function that returns sum of the previous values

We will see how to create a javascript function that will remember its previously passed values and return the sum of the current and previous value.

---

> [!example] Example
> 
> ```javascript
> sum(5); // 5
> sum(3); // 8
> sum(4); // 12
> sum(0); // 12
> ```
> 
> - The first call `sum(5)` returns `5` (since it’s the first value).
>     
> - The second call `sum(3)` adds it to the previous value → `5 + 3 = 8`.
>     
> - And so on.
>     

---

To create such functions we should know how the javascript function works. Javascript functions have access to the state (properties & methods) of its parent function even after it is executed.

So to create a function that will return the sum of the previous values in javascript we will use this technique of **closure**.

---

## Implementation

```javascript
const curry = () => {
  // To store the previous values
  let sum = 0;
  
  // Return an inner function 
  // Which will have access to its parent function's store
  return function(num = 0) {
    sum += num;
    return sum;
  };
};
```

Here we have created a function named `curry` which has a store to keep track of the previous value and returns an inner function.

The powerful feature of the javascript function is that the inner function still has access to the parent function’s properties and methods even after it is returned.

We now call this function which will return the inner function and we can use the returned inner function for our action.

```javascript
// Returns and stores the inner function.
let sum = curry();
```

Now if we call this with the specified value, it should return the desired output.

```javascript
console.log(sum(5)); // 5
console.log(sum(3)); // 8
console.log(sum(4)); // 12
console.log(sum(0)); // 12
console.log(sum());  // 12
```

---

> [!tip] Key Point
> 
> ```javascript
> // Why this works
> // Closures allow the inner function to keep a reference
> // to 'sum' from the parent function even after 'curry' has finished execution.
> ```
> 
> This works because of **closures** — the inner function keeps a reference to the variable `sum` from the parent function even after `curry` has finished execution.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/QJkltlNkmqs)

---

> [!summary] Takeaway  
> Use **closures** to persist state between function calls — allowing functions to “remember” values across executions.

---
