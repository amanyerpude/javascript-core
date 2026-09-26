
---

> [!quote] Metadata  
> **Posted on:** June 7, 2021  
> **Author:** 
> **Posted in:** Interview, JavaScript  
> **Tags:** #relations #tree #reduce #recursion #arrays-to-object

---

# Convert Entity-Relation Array to Object in JavaScript

Given an array with **two entries per row**—`[child, parent]`—convert it into a **relation tree** expressed as strings (`parent -> child -> grandchild`) in JavaScript.

The input array can contain relations for **many ancestries** in **random order**. We must return an **array of strings** representing different relationships.  
For example, in the cases below, the **topmost ancestor** is `animal`.

---

## Example

**Input:**

```javascript
[
  ["lion", "cat"],
  ["cat", "mammal"],
  ["dog", "mammal"],
  ["mammal", "animal"],
  ["fish", "animal"],
  ["shark", "fish"],
];
```

**Output:**

```javascript
[
  "animal -> mammal -> cat -> lion",
  "animal -> mammal -> cat",
  "animal -> mammal -> dog",
  "animal -> mammal",
  "animal -> fish",
  "animal -> fish -> shark"
]
```

---

## Concept

### Convert relation array to object tree in JavaScript

The first thing we have to do is to **convert the array** to an **object of the parent–child relationship** for better processing.

```javascript
// aggregate parent / child relation
const aggregate = (arr) => {
  
  // aggregate the values for easier processing
  return arr.reduce((a, b) => {
    const [child, parent] = b;
    
    // aggregating on child
    a[child] = parent;
    
    return a;
  }, {});
}

const arr = [
  ["lion", "cat"],
  ["cat", "mammal"],
  ["dog", "mammal"],
  ["mammal", "animal"],
  ["fish", "animal"],
  ["shark", "fish"],
];

console.log(aggregate(arr));

/*
{
  "lion": "cat",
  "cat": "mammal",
  "dog": "mammal",
  "mammal": "animal",
  "fish": "animal",
  "shark": "fish"
}
*/
```

We have aggregated the values **on the child** (one child will have only one parent). This way a single key–value map is formed, which is **faster for forming relationships**.

---

Now, all we have to do is **recursively** keep traversing this map and **form the relation string**. For this, we will get the value of the current key and recursively call the same function with this value (its parent) and the map to get its ancestry and so on.

```javascript
// for a relationship from the aggregated value
const convert = (obj) => {
  return Object.keys(obj).reduce((a, b) => {
    a.push(getKey(obj, b));
    return a;
  }, []);
}

// helper function to form the string
// till the last hierarchy
const getKey = (obj, key) => {
  // access the
  const val = obj[key];
  
  // the formation can be reversed by chaning the order of the keys
  // child -> parent | parent -> child
  if(val in obj){
    return getKey(obj, val) + " -> " + key;
  }else{
    return val + " -> " + key;
  }
}
```

**Input:**

```javascript
// map after aggregation
const map = {
  "lion": "cat",
  "cat": "mammal",
  "dog": "mammal",
  "mammal": "animal",
  "fish": "animal",
  "shark": "fish"
};

console.log(convert(map));
```

**Output:**

```javascript
[
  "animal -> mammal -> cat -> lion",
  "animal -> mammal -> cat",
  "animal -> mammal -> dog",
  "animal -> mammal",
  "animal -> fish",
  "animal -> fish -> shark"
]
```

---

## Complete Code

```javascript
const ancestry = (arr) => {

  // aggregate parent / child relation
  const aggregate = (arr) => {

    // aggregate the values for easier processing
    return arr.reduce((a, b) => {
      const [child, parent] = b;

      // aggregating on child
      a[child] = parent;

      return a;
    }, {});
  };
  
  // for a relationship from the aggregated value
  const convert = (obj) => {
    return Object.keys(obj).reduce((a, b) => {
      a.push(getKey(obj, b));
      return a;
    }, []);
  };

  // helper function to form the string
  // till the last hierarchy
  const getKey = (obj, key) => {
    // access the
    const val = obj[key];

    // the formation can be reversed by chaning the order of the keys
    // child -> parent | parent -> child
    if(val in obj){
      return getKey(obj, val) + " -> " + key;
    }else{
      return val + " -> " + key;
    }
  };
  
  // get the aggregated map
  const aggregatedMap = aggregate(arr);
  
  // return the ancestory 
  return convert(aggregatedMap);

};
```

**Input:**

```javascript
const arr = [
  ["lion", "cat"],
  ["cat", "mammal"],
  ["dog", "mammal"],
  ["mammal", "animal"],
  ["fish", "animal"],
  ["shark", "fish"],
];

console.log(ancestry(arr));
```

**Output:**

```javascript
[
  "animal -> mammal -> cat -> lion",
  "animal -> mammal -> cat",
  "animal -> mammal -> dog",
  "animal -> mammal",
  "animal -> fish",
  "animal -> fish -> shark"
]
```

---

> [!tip] Key Insights
> 
> - **O(n)** aggregation using `reduce()` builds a quick lookup map: `child → parent`.
>     
> - A **recursive walk** from any child to its root forms the **full lineage** string.
>     
> - If you need the reverse order (`child -> parent`), simply flip the concatenation.
>     
> - If input can contain **multiple ancestries**, the approach still works—each child resolves to its own root.
>     
> - For robustness in production, consider handling **cycles** (e.g., `A→B`, `B→A`) and **orphans** (children without parents).
>     

---

> [!summary] Takeaway  
> Converting `[child, parent]` pairs into readable **ancestry chains** is a two-step pattern:
> 
> 1. **Aggregate** to a `child → parent` map,
>     
> 2. **Recurse** to construct full **`root -> ... -> child`** paths.  
>     This pattern pops up in **org charts**, **category hierarchies**, **routing trees**, and **dependency graphs**.
>     

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/convert-entity-relation-array-to-object-in-javascript/)

---
