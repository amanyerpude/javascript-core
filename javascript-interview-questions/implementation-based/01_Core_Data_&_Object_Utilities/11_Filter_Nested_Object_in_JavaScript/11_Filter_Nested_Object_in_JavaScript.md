
---

> [!quote] Metadata  
> **Posted on:** June 7, 2021  
> **Author:**   
> **Posted in:** Interview, JavaScript  
> **Tags:** #filter #nested #object #recursion #inplace

---

# Filter Nested Object in JavaScript

Create a function in JavaScript which takes:

- a **nested object**, and
    
- a **filter function**
    

and returns a filtered object.

The important clause: **filtering should happen in-place**. We cannot create a new object; we must modify the original.

---

## Example

**Input:**

```javascript
const obj = {
  a: 1,
  b: {
    c: "Hello World",
    d: 2,
    e: {
     f: {
       g: -4,
      },
    },
    h: "Good Night Moon",
  },
};

const filter = (s) => typeof s === "string";
```

**Output:**

```javascript
{
  b: {
    c: "Hello World",
    h: "Good Night Moon",
  }
}
```

---

## Concept

To filter the object in-place:

1. Iterate all entries of the object.
    
2. For each key:
    
    - If the value is an **object**, recursively call the same function.
        
    - If the value is a **primitive**, check it with the filter:
        
        - If it **fails**, delete the key.
            
3. After recursion, if the value is an **empty object**, delete the key.
    

We detect empty objects by checking:

```javascript
JSON.stringify(val) === "{}"
```

---

## Implementation

```javascript
const deepFilter = (obj, filter) => {
  // iterate the object
  for (let key in obj) {
    const val = obj[key];

    // if val is also an object (nested)
    if (typeof val === "object") {
      // recur
      deepFilter(val, filter);
    } 
    // primitive value
    else {
      // fails filter condition → delete
      if (filter(val) === false) {
        delete obj[key];
      }
    }

    // if value became an empty object → delete it
    if (JSON.stringify(val) === "{}") {
      delete obj[key];
    }
  }
};
```

---

## Demo

**Input:**

```javascript
const obj = {
  a: 1,
  b: {
    c: "Hello World",
    d: 2,
    e: {
     f: {
       g: -4,
      },
    },
    h: "Good Night Moon",
  },
};

// filter in-place
deepFilter(obj, (s) => typeof s === "string");

console.log(obj);
```

**Output:**

```javascript
{
  b: {
    c: "Hello World",
    h: "Good Night Moon",
  }
}
```

---

> [!tip] Key Insights
> 
> - Works **recursively** to handle deeply nested structures.
>     
> - Filtering is done **in-place**: no new object is created.
>     
> - Empty objects are **cleaned up** automatically.
>     
> - The filter function can be customized (e.g., numbers > 0, non-null values, etc.).
>     

---

> [!summary] Takeaway  
> `deepFilter` is a flexible recursive utility for:
> 
> - Filtering **deeply nested JSON structures**
>     
> - Cleaning up unwanted values (e.g., `null`, `undefined`, negatives)
>     
> - Ensuring the resulting object only contains keys matching your filter criteria
>     
> 
> A practical **interview question** that strengthens your understanding of **recursion, in-place mutation, and object traversal** in JavaScript.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/XMEYVsxelxk)

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/filter-nested-object-in-javascript/)

---
