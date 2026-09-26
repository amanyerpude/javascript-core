
---

> [!quote] Metadata  
> **Posted on:** June 27, 2023  
> **Author:** 
> **Posted in:** Interview, Javascript

---

# Piping function in JavaScript – Part 2

Create a function that accepts multiple functions as arguments and a value, runs the value through each function in sequence, and returns the final output.

Also checkout → [[Piping function in JavaScript – Part 1]]

---

> [!example] Example  
> **Input:**
> 
> ```javascript
> const val = { salary: 10000 };
> 
> const getSalary = (person) => person.salary
> const addBonus = (netSalary) => netSalary + 1000;
> const deductTax = (grossSalary) => grossSalary - (grossSalary * .3);
> 
> const result = pipe(
>   getSalary,
>   addBonus,
>   deductTax 
> )(val);
> ```
> 
> **Output:**
> 
> ```javascript
> 7700
> ```

---

## Concept

The best way to solve this is by forming a **closure**:

1. Outer function accepts all functions as arguments.
    
2. Inner function accepts the value.
    
3. Run the value through the sequence of functions.
    
4. Return the final result from the last function.
    

---

## Implementation

```javascript
// accept functions as arguments
// using rest ... operator convert them to array
const pipe = function(...fns) {
  
  // form a closure with inner function
  return function(val) {
    // run the value through all the functions
    for (let f of fns) {
      val = f(val);
    }
    
    // return the value after last processing
    return val;
  };
};
```

---

## Input / Output

```javascript
const getSalary = (person) => person.salary
const addBonus = (netSalary) => netSalary + 1000;
const deductTax = (grossSalary) => grossSalary - (grossSalary * .3);

const val = { salary: 10000 };

const result = pipe(
  getSalary,
  addBonus,
  deductTax 
)({ salary: 10000 });

console.log(result);
```

**Output:**

```javascript
7700
```

---

> [!tip] Key Insight
> 
> - This works because of **closures** (outer function stores the list of functions).
>     
> - The inner function runs the value through each function sequentially.
>     
> - This is essentially a **function composition** pattern.
>     

---

> [!summary] Takeaway  
> A **piping function** enables **function composition**: passing a value through multiple functions in sequence to produce a final transformed result.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/jL6fLgf9Urw)

---
