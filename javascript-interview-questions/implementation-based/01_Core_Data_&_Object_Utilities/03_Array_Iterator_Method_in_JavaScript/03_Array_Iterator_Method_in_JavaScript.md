
---

> [!quote] Metadata  
> **Posted on:** March 22, 2023  
> **Author:**   
> **Posted in:** Interview, JavaScript  
> **Tags:** #iterator #closure #array #helper #interview

---

# Array Iterator Method in JavaScript

Create a custom **iterator method** that accepts an array and returns a function (or object) capable of returning the **next array value** on each invocation, along with a way to check if iteration is complete.

This mimics the behavior of built-in JavaScript iterators but helps you understand **closures, state management, and iteration protocols** in depth.

---

## Example

```javascript
let iterator = helper([1, 2, "hello"]);

console.log(iterator.next()); // 1
console.log(iterator.next()); // 2
console.log(iterator.done()); // false
console.log(iterator.next()); // "hello"
console.log(iterator.done()); // true
console.log(iterator.next()); // null
```

---

## Concept

We can implement this helper function by:

1. **Forming a closure** around the array and an index tracker.
    
2. Returning an object with two methods:
    
    - **`next()`** → returns the next array element or `null` if finished.
        
    - **`done()`** → returns a boolean (`true` if all values have been returned).
        
3. Using the index tracker to keep state across calls.
    

This is a simplified form of how **iterators** and the **iteration protocol** work in ES6+.

---

## Implementation

```javascript
const helper = (array) => {
  // track the index
  let nextIndex = 0;
  
  // return two methods
  return {
    // return the next value or null
    next: function () {
      return nextIndex < array.length
        ? array[nextIndex++]    
        : null;
    },
    
    // returns boolean value if iteration is complete
    done: function () {
      return nextIndex >= array.length;
    }
  };
};
```

---

## Demo

```javascript
let iterator = helper([1, 2, "hello"]);

console.log(iterator.next()); // 1
console.log(iterator.next()); // 2
console.log(iterator.done()); // false
console.log(iterator.next()); // "hello"
console.log(iterator.done()); // true
console.log(iterator.next()); // null
```

---

## Alternative: Using ES6 Generators

While closures work perfectly, modern JavaScript provides **generators** (`function*`) to simplify iterator creation:

```javascript
function* generatorHelper(array) {
  for (let item of array) {
    yield item;
  }
}

const iterator = generatorHelper([1, 2, "hello"]);

console.log(iterator.next().value); // 1
console.log(iterator.next().value); // 2
console.log(iterator.next().done);  // false
console.log(iterator.next().value); // "hello"
console.log(iterator.next().done);  // true
```

This is much closer to the **native iterator protocol** that powers constructs like `for...of`.

---

> [!tip] Key Insights
> 
> - The closure-based `helper` maintains iteration state **privately**.
>     
> - `next()` moves the cursor forward, `done()` signals completion.
>     
> - Mimics the **Iterator Protocol** in a simplified way.
>     
> - ES6 **generators** provide a built-in and more powerful alternative.
>     
> - Great interview question: demonstrates closures, encapsulation, and iteration logic.
>     

---

> [!summary] Takeaway  
> Building a custom **array iterator** helps you:
> 
> - Understand how iteration works under the hood.
>     
> - Practice closures and stateful functions.
>     
> - Compare closures vs. ES6 generators for iteration.
>     
> 
> This pattern is essential for mastering **iterables, async iterables, and custom data structures** in JavaScript.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/jYmG4nbrmOw)

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/array-iterator-method-in-javascript/)

---
