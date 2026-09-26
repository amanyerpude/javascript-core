
---

> [!quote] Metadata  
> **Posted on:** September 29, 2022  
> **Author:** 
> **Posted in:** Interview, Javascript

---

# Currying – Part 3

Write a function that satisfies the following:

> [!example] Example
> 
> ```javascript
> add(1)(2).value() = 3; 
> add(1, 2)(3).value() = 6; 
> add(1)(2)(3).value() = 6; 
> add(1)(2) + 3 = 6;
> ```

This question is part of the **currying methodology in JavaScript**.

- [[Javascript function that returns sum of the previous values]]
    
- [[Currying in JavaScript]]
    

---

Currying in JavaScript is a concept of functional programming in which we can pass functions as arguments (callbacks) and return functions without any side effects (Changes to program states).

This one is a little tricky and requires us to use and modify the **`valueOf()`** method.

---

> [!tip] About `valueOf()`  
> When JavaScript wants to turn an object into a primitive value, it uses the function `valueOf()` method.
> 
> - JavaScript automatically calls `valueOf()` when it comes across an object where a primitive value is anticipated.
>     
> - This means you don’t even need to call it yourself.
>     

---

## Example

```javascript
function MyNumberType(n) {
  this.number = n;
}

MyNumberType.prototype.valueOf = function () {
  return this.number + 1;
};

const myObj = new MyNumberType(4);
myObj + 3; // 8
```

---

Thus we can form a closure and track the arguments in an array, returning a new function every time to accept new arguments.

We also override the **`valueOf()`** method and return the sum of all arguments for each primitive action.  
Additionally, we add a method **`.value()`** that references `valueOf()`, so invoking it will return the sum of arguments.

---

## Implementation

```javascript
function add(...x) {
  // store the current arguments
  let sum = x;

  function resultFn(...y) {
    // merge the new arguments
    sum = [...sum, ...y];
    return resultFn;
  }
  
  // override valueOf to return sum
  resultFn.valueOf = function() {
    return sum.reduce((a, b) => a + b, 0);
  };
  
  // extend valueOf with value()
  resultFn.value = resultFn.valueOf;
  
  // return the inner function
  // on any primitive action, valueOf will be invoked
  return resultFn;
}
```

---

## Input / Output

```javascript
console.log(add(1)(2).value() == 3); 
console.log(add(1, 2)(3).value() == 6); 
console.log(add(1)(2)(3).value() == 6); 
console.log(add(1)(2) + 3);

// Output:
true
true
true
6
```

---

> [!summary] Takeaway  
> By combining **closures** with an overridden **`valueOf()`** method, we can implement flexible currying functions in JavaScript that:
> 
> - Chain arguments dynamically
>     
> - Return correct values in both `.value()` calls and primitive operations (like `+`)
>     

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/qONGx06Z4_A)

---
