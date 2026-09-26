
---

> [!quote] Metadata  
> **Posted on:** April 21, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #object #cycle

---

# Remove Cycle from the Object

Given an object with a cycle, remove the cycle or circular reference from it.

---

## Example

```javascript
const List = function (val) {
  this.next = null;
  this.val = val;
};

const item1 = new List(10);
const item2 = new List(20);
const item3 = new List(30);
item1.next = item2;
item2.next = item3;
item3.next = item1; // creates cycle

removeCycle(item1);
console.log(item1);
// Output: { val: 10, next: { val: 20, next: { val: 30 } } }
```

---

## Approach 1: Using WeakSet (Normal Use)

```javascript
const removeCycle = (obj) => {
  const set = new WeakSet([obj]);

  (function iterateObj(obj) {
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        if (typeof obj[key] === 'object') {
          if (set.has(obj[key])) {
            delete obj[key];
          } else {
            set.add(obj[key]);
            iterateObj(obj[key]);
          }
        }
      }
    }
  })(obj);
};
```

---

## Approach 2: Using JSON.stringify Replacer

```javascript
const getCircularReplacer = () => {
  const seen = new WeakSet();
  return (key, value) => {
    if (typeof value === 'object' && value !== null) {
      if (seen.has(value)) {
        return;
      }
      seen.add(value);
    }
    return value;
  };
};

// Usage
console.log(JSON.stringify(item1, getCircularReplacer()));
// Output: "{'next':{'next':{'val':30},'val':20},'val':10}"
```

---

## How It Works

### WeakSet Approach
1. Create a `WeakSet` to store visited object references.
2. Recursively iterate through object properties.
3. If an object reference is already in the set → **delete** it (break cycle).
4. Otherwise, **add** it to the set and continue recursion.

### JSON.stringify Approach
1. Use a `WeakSet` in a closure to track seen objects.
2. Return `undefined` for circular references (removes them from output).
3. Add each object to the set when first encountered.

---

> [!tip] Key Points
> - **WeakSet** stores only object references and allows garbage collection.
> - The WeakSet approach modifies the object **in place**.
> - The JSON.stringify approach creates a **string representation** without cycles.
> - Both approaches use `WeakSet` for O(1) lookup of visited objects.

---

> [!summary] Takeaway
> - Detect cycles using **WeakSet** to track visited object references.
> - **WeakSet approach**: modifies object in place, deletes cyclic references.
> - **JSON.stringify approach**: uses replacer function to skip circular refs.
> - Useful for **serialization** and **debugging** circular structures.

---

### 📎 Reference

[Remove cycle from the object - LearnersBucket](https://learnersbucket.com/examples/interview/remove-cycle-from-the-object-in-javascript/)

---
