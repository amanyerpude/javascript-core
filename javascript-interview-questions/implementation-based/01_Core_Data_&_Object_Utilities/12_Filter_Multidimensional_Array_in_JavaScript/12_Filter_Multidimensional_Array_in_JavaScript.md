
---

> [!quote] Metadata  
> **Posted on:** February 22, 2022  
> **Author:** 
> **Posted in:** Interview, JavaScript  
> **Tags:** #multidimensional #filter #recursion #prototype #function

---

# Filter Multidimensional Array in JavaScript

Given a **multi-dimensional array**, the task is to create a filter function that takes a **callback function** as input and returns a **new array** with all elements that pass the test implemented in the callback.

This problem is a recursive extension of the native `Array.prototype.filter()` and is often used to test recursion and array manipulation in **interviews**.

---

## Example 1

**Input:**

```javascript
const arr = [[1, [2, [3, 'foo', { 'a': 1, 'b': 2 }]], 'bar']];
const filtered = arr.multiFilter((e) => typeof e === 'string');
console.log(filtered);
```

**Output:**

```javascript
[[[["foo"]],"bar"]]
```

---

## Approach

To filter a **multi-dimensional array**, we must:

1. Iterate over every element in the array.
    
2. If the element is itself an array, **recursively filter** it.
    
3. If the element is not an array, apply the **test function** (callback).
    
    - If it passes, keep it.
        
    - If it fails, skip it.
        
4. Collect all results in a new array.
    

---

## Recursive Function

```javascript
const filter = (arr, test) => {
  // Store the output
  const result = [];

  // iterate the array
  for (let a of arr) {
    // if sub-array
    if (Array.isArray(a)) {
      // recursively filter the sub-array
      const output = filter(a, test);

      // store the result
      result.push(output);
    } else {
      // if not an array → test the element
      if (test(a)) {
        result.push(a);
      }
    }
  }

  // return the result
  return result;
};
```

---

## Polyfill for `multiFilter`

We’ve already seen how to create a polyfill for `Array.prototype.filter()` in a 1D array.  
Now let’s **extend Array’s prototype** with a new method `multiFilter()`:

```javascript
Array.prototype.multiFilter = function (test) {
  // original array
  const originalArray = this;

  const filter = (arr, test) => {
    const result = [];

    for (let a of arr) {
      if (Array.isArray(a)) {
        const output = filter(a, test);
        result.push(output);
      } else {
        if (test(a)) {
          result.push(a);
        }
      }
    }

    return result;
  };

  // filter and return
  return filter(originalArray, test);
};
```

---

## Example 2

**Input:**

```javascript
const arr = [[1, [2, [3, "foo", { a: 1, b: 2 }]], "bar"]];
const filtered = arr.multiFilter((e) => typeof e === "number");
console.log(JSON.stringify(filtered));
```

**Output:**

```javascript
[[1,[2,[3]]]]
```

---

> [!tip] Key Insights
> 
> - This extends the **filtering paradigm** to **nested arrays**.
>     
> - Works recursively for **any depth** of nesting.
>     
> - Keeps the **structure intact** while filtering out unwanted elements.
>     
> - The callback function is flexible: you can filter by type, range, regex, etc.
>     
> - Unlike flat arrays, here **non-matching elements are simply omitted**, but the nesting remains.
>     

---

> [!summary] Takeaway  
> `multiFilter` is a **recursive polyfill** that behaves like `filter()` but for multi-dimensional arrays:
> 
> - Traverses **deeply nested structures**.
>     
> - Applies filtering logic consistently at all levels.
>     
> - Retains **nested array shape** in the result.
>     
> 
> This is a common **interview exercise** testing recursion, prototypes, and array manipulation in JavaScript.

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/filter-multidimensional-array-in-javascript/)

---
