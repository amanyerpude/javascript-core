
---

> [!quote] Metadata  
> **Posted on:** June 12, 2021  
> **Author:**  
> **Posted in:** Interview, Javascript

---

# Currying in JavaScript

Currying in JavaScript is a concept of functional programming in which we can pass functions as arguments (callbacks) and return functions without any side effects (Changes to program states).

---

## What is currying?

In simple terms, in currying we return a function for each function invoked which accepts the next argument inline. With the help of currying we can transform a function with multiple arguments into a sequence of nesting functions.

---

> [!example] Example
> 
> ```javascript
> // normal function
> sum(1, 2, 3)
> // should return 6
> 
> // currying function
> sum(1)(2)(3)
> // should return 6
> ```
> 
> If you notice in the currying function, for each function call `sum(1)` we are returning a function which accepts the next argument `sum(1)(2)` and it again returns a function which accepts argument `sum(1)(2)(3)` and so on.

There is no specified limit to how many times you can return a function.  
There are also different variations to currying, such as the first function accepting 2 arguments, the next function any number of arguments, and so on.

---

> [!example] Variations of currying
> 
> ```javascript
> sum(1)(2)(3)
> sum(1, 2)(3)
> sum(1)(2, 3)
> sum(1, 2, 3)
> 
> // all of them should return the same output → 6
> ```

Now you may be wondering: if each function call returns a new function, how is the value returned?  
For that, we must define a **base condition** — for example:

- If no argument is passed in the next function call, return the value.
    
- Or if we have reached 5 arguments, return the value.
    

---

> [!example] Base condition examples
> 
> ```javascript
> sum(1)(2)(3)()
> sum(1, 2)(3)()
> sum(1)(2, 3)()
> sum(1, 2, 3)()
> // all should return 6
> 
> // OR return when we reach 5 arguments
> sum(1, 2, 3, 4, 5)
> sum(1, 2)(3, 4, 5)
> sum(1)(2, 3, 4, 5)
> sum(1, 2, 3)(4, 5)
> sum(1)(2)(3)(4)(5)
> sum(1, 2, 3, 4)(5)
> // all should return 15
> ```

---

I was recently asked this in an interview: **implement the currying function for 4 arguments**.  
When the limit is reached, return the value.

**Note:** Maximum 4 arguments will only be passed.

---

> [!example] Interview case (limit 4 arguments)
> 
> ```javascript
> sum(1, 2, 3, 4)
> sum(1)(2)(3)(4)
> sum(1, 2)(3, 4)
> sum(1, 2, 3)(4)
> sum(1)(2, 3, 4)
> 
> // all should return 10
> ```

---

## Implementation

First, let’s handle the base case:

```javascript
const sum = (...args) => {
  // spread the arguments in storage array
  const storage = [...args];
  
  // base case
  if (storage.length === 4) {
    return storage.reduce((a, b) => a + b, 0);
  }
}
```

- `...args` is the rest operator, which aggregates all the passed arguments into an array.
    
- If `storage.length === 4`, return the sum.
    
- Otherwise, return a new function until the limit is reached.
    

---

```javascript
const sum = (...args) => {
  const storage = [...args];
  
  if (storage.length === 4) {
    return storage.reduce((a, b) => a + b, 0);
  } else {
    const temp = function(...args2) {
      storage.push(...args2);

      if (storage.length === 4) {
        return storage.reduce((a, b) => a + b, 0);
      } else {
        return temp;
      }
    }
    return temp;
  }
}
```

---

> [!tip] Closure reminder  
> According to MDN:  
> _A closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time._
> 
> This allows us to keep accessing the `storage` array across nested function calls.

---

## Input / Output

```javascript
const res = sum(1, 2, 3, 4);
const res2 = sum(1)(2)(3)(4);
const res3 = sum(1, 2)(3, 4);
const res4 = sum(1, 2, 3)(4);
const res5 = sum(1)(2, 3, 4);

console.log(res, res2, res3, res4, res5);
// Output: 10 10 10 10 10
```

---

## Handling empty arguments

Interviewer follow-up: **return the value when invoked with empty arguments.**

```javascript
sum(1, 2, 3, 4)();
sum(1)(2)(3)(4)();
sum(1, 2)(3, 4)();
sum(1, 2, 3)(4)();
sum(1)(2, 3, 4)();
sum();

// Output: 10 10 10 10 10 0
```

---

## Final Implementation

```javascript
const sum = (...args) => {
  const storage = [...args];
  
  // base case → no arguments
  if (storage.length === 0) {
    return 0;
  } else {
    const temp = function(...args2) {
      storage.push(...args2);

      if (args2.length === 0) {
        return storage.reduce((a, b) => a + b, 0);
      } else {
        return temp;
      }
    }
    return temp;
  }
}
```

---

## Final Input / Output

```javascript
const res = sum(1, 2, 3, 4)();
const res2 = sum(1)(2)(3)(4)();
const res3 = sum(1, 2)(3, 4)();
const res4 = sum(1, 2, 3)(4)();
const res5 = sum(1)(2, 3, 4)();
const res6 = sum();

console.log(res, res2, res3, res4, res5, res6);
// Output: 10 10 10 10 10 0
```

---

> [!summary] Takeaway  
> Currying transforms a multi-argument function into nested functions that take one (or fewer) arguments at a time.  
> Closures allow these functions to **remember previous arguments**, enabling flexible invocation styles and base conditions.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/7me5z_QNN1M)

---
