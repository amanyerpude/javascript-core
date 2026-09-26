
---

> [!quote] Metadata  
> **Posted on:** October 3, 2022  
> **Author:**   
> **Posted in:** Interview, JavaScript  
> **Tags:** #aggregate #reduce #array #object #groupBy

---

# Aggregate Array of Objects on Given Keys in JavaScript

Given an **array of objects** and two keys `on` and `who`, the task is to **aggregate** the `who` values based on the `on` values.

This is a common **data transformation problem** that comes up in interviews and real-world data manipulation scenarios (e.g., grouping endorsements, survey results, votes, etc.).

---

## Example 1

**Input:**

```javascript
const endorsements = [ 
  { skill: 'css', user: 'Bill' }, 
  { skill: 'javascript', user: 'Chad' }, 
  { skill: 'javascript', user: 'Bill' }, 
  { skill: 'css', user: 'Sue' }, 
  { skill: 'javascript', user: 'Sue' }, 
  { skill: 'html', user: 'Sue' } 
];

console.log(aggregate(endorsements, "user", "skill"));
```

**Output:**

```javascript
[
  {
    "user": "Bill",
    "skill": ["css", "javascript"]
  },
  {
    "user": "Chad",
    "skill": ["javascript"]
  },
  {
    "user": "Sue",
    "skill": ["css", "javascript", "html"]
  }
]
```

---

## Approach

We’ll use **`Array.prototype.reduce()`** to build a grouped structure:

1. Loop over the input array.
    
2. For each element:
    
    - Get the `on` value (grouping key).
        
    - Get the `who` value (aggregated key).
        
3. If the `on` value already exists in the accumulator → append the new `who` value.
    
4. Else → create a new entry with the `who` value in an array.
    
5. Finally, return **only the values** of the accumulator object (discard the grouping keys).
    

---

## Implementation

```javascript
const aggregate = (arr, on, who) => {
  // using reduce() method to aggregate 
  const agg = arr.reduce((a, b) => {
    // get the value of both the keys 
    const onValue = b[on];
    const whoValue = b[who];
    
    // if there is already a key present
    // merge its value
    if (a[onValue]) {
      a[onValue] = {
        [on]: onValue,
        [who]: [...a[onValue][who], whoValue]
      };
    }
    // create a new entry on the key
    else {
      a[onValue] = {
        [on]: onValue,
        [who]: [whoValue]
      };
    }
    
    return a;
  }, {});
  
  // return only values after aggregation 
  return Object.values(agg);
};
```

---

## Example 2

**Input:**

```javascript
const endorsements = [ 
  { skill: 'css', user: 'Bill' }, 
  { skill: 'javascript', user: 'Chad' }, 
  { skill: 'javascript', user: 'Bill' }, 
  { skill: 'css', user: 'Sue' }, 
  { skill: 'javascript', user: 'Sue' }, 
  { skill: 'html', user: 'Sue' } 
];

console.log(aggregate(endorsements, "skill", "user"));
```

**Output:**

```javascript
[
  {
    "skill": "css",
    "user": ["Bill", "Sue"]
  },
  {
    "skill": "javascript",
    "user": ["Chad", "Bill", "Sue"]
  },
  {
    "skill": "html",
    "user": ["Sue"]
  }
]
```

---

> [!tip] Key Insights
> 
> - This is essentially a **groupBy operation**, aggregating values by a chosen key.
>     
> - `reduce()` works well for **single-pass transformations**.
>     
> - If duplicates need to be removed, wrap `[...a[onValue][who], whoValue]` with `new Set(...)`.
>     
> - Flexible: you can aggregate on **any property** (e.g., group votes by candidate, sales by region).
>     

---

> [!summary] Takeaway  
> Using `reduce()` to aggregate arrays is a powerful pattern:
> 
> - It creates **maps of grouped data**.
>     
> - It transforms data into the desired **reporting/visualization format**.
>     
> - It’s reusable: swap the keys to **pivot the grouping logic**.
>     
> 
> This is a common **interview question** because it tests knowledge of `reduce()`, objects, and array transformations.

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/aggregate-array-of-objects-on-the-given-keys/)

---
