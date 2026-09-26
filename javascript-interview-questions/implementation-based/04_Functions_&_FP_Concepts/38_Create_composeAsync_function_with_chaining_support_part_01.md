
YouTube Video : [Compose async functions with chaining support | Adobe MTS2 JavaScript interview question](https://www.youtube.com/@Learnersbucket)
This interview question was asked in Adobe's Frontend Interview for the MTS-2 level. It is a challenging JavaScript question that evaluates your understanding of closures and asynchronous operations.

Part 2 of the above : https://youtu.be/zVvIn9aZLos?si=xZpPJ2lfWQPSmGUU


--------------------------------------------------------------------------

> [!quote] Metadata  
> **Asked in:** Adobe MTS2 Front-end Engineer Interview  
> **Posted in:** Interview, Javascript  
> **Tags:** #function #async #compose

---

# Create Compose Async Functions with Chaining Support

The goal is to create a **`composeAsync`** function that supports chaining.

---

## Problem Statement

- `composeAsync` accepts multiple functions as arguments (in reverse order).
    
- It returns another function that accepts the inputs.
    
- The returned function executes the functions sequentially (right to left), passing results from one into the next.
    
- Each function returns a **Promise**, so chaining `.then()` and `.catch()` works.
    

---

> [!example] Example
> 
> ```javascript
> // Functions A, B, and C
> const A = (a, b) => new Promise(res => setTimeout(() => res(a * b), 100));
> const B = (x) => new Promise(res => setTimeout(() => res(x + 5), 100));
> const C = (x) => new Promise(res => setTimeout(() => res(x / 10), 100));
> 
> // Compose async
> const result = composeAsync(C, B, A)(5, 3);
> 
> result.then(console.log).catch(console.error);
> // Expected output: 2
> ```

---

## Concept

- **Closures:** Returned async function remembers the list of functions.
    
- **Compose vs Pipe:**
    
    - `compose` → executes right → left (last function is applied first).
        
    - `pipe` → executes left → right.
        
- **Async Execution:**
    
    - Each function returns a Promise.
        
    - Use `await` to ensure sequential execution.
        

---

## Implementation

```javascript
const composeAsync = (...fns) => {
  // return an async function that accepts inputs
  return async (...inputs) => {
    // reverse order for compose
    const functions = [...fns].reverse();
    let result = inputs;

    for (let fn of functions) {
      // ensure inputs are passed correctly
      result = Array.isArray(result) ? await fn(...result) : await fn(result);
    }

    return result;
  };
};
```

---

## Input / Output

```javascript
// Define sample functions
const A = (a, b) => new Promise(res => setTimeout(() => res(a * b), 100));
const B = (x) => new Promise(res => setTimeout(() => res(x + 5), 100));
const C = (x) => new Promise(res => setTimeout(() => res(x / 10), 100));

// Test
composeAsync(C, B, A)(5, 3)
  .then(result => console.log("Final Result:", result))
  .catch(err => console.error("Error:", err));
```

**Execution Flow:**

1. `A(5,3)` → `15`
    
2. `B(15)` → `20`
    
3. `C(20)` → `2`
    

**Output:**

```javascript
Final Result: 2
```

---

> [!tip] Key Insights
> 
> - `composeAsync` evaluates functions **right to left**.
>     
> - Works seamlessly with **Promises** thanks to `async/await`.
>     
> - Demonstrates knowledge of **closures**, **function composition**, and **asynchronous JavaScript** — exactly what interviewers test.
>     

---

> [!summary] Takeaway  
> `composeAsync` lets you build complex async pipelines by chaining multiple functions, executing them in sequence, and returning a Promise.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/rWFc0Nc1VNU?si=FNnVcs3SFAxgpgwg)

---
