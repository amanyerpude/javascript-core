
---

> [!quote] Metadata  
> **Posted on:** June 26, 2023  
> **Author:** 
> **Posted in:** Interview, JavaScript  
> **Tags:** #form #aggregate #reduce #object #nested

---

# Aggregate the Input Values in JavaScript

Given multiple **input elements** with different `name` attributes and `value`s inside a wrapper (e.g., a form), the goal is to **aggregate the values based on their names** into a **nested object structure**.

This problem is a practical interview question and also useful in **real-world form handling**.

---

## Example

**Input (HTML):**

```html
<form id="parent">
  <input type="text" name="a.c" value="1"/>
  <input type="text" name="a.b.d" value="2"/>
  <input type="text" name="a.b.e" value="3"/>
</form>
```

**Output (JavaScript Object):**

```javascript
{
  "a": {
    "c": "1",
    "b": {
      "d": "2",
      "e": "3"
    }
  }
}
```

---

## Approach

1. **Target the parent wrapper** using `document.querySelector`.
    
2. **Collect input elements** (`input[type="text"]`) under it.
    
    - This can be generalized to other input types if needed.
        
3. Use **`Array.reduce()`** to aggregate each input’s value into a nested object:
    
    - Split the `name` on `"."` to get keys as an array.
        
    - Traverse the object hierarchy step by step.
        
    - If a key does not exist, create an empty object.
        
    - If it’s the last key, assign the input’s value.
        
    - Update the reference as you go deeper.
        

---

## Implementation

```javascript
const aggregateValues = (id) => {
  // get the element
  const element = document.querySelector(`#${id}`);
  
  // get all the input elements under it
  // change the type depending on the requirement
  const inputs = element.querySelectorAll('input[type="text"]');
  
  // aggregate the input values
  return Array.from(inputs).reduce((prev, current) => {
    // split the name to form an array of keys
    const names = current.name.split(".");
    
    // reference for traversing the object (parent -> child -> grandchild)
    let temp = prev;
    
    // iterate each key in the name
    names.forEach((name, index) => {
      // if the key is not already present, create an empty object
      if (!(name in temp)) {
        temp[name] = {};
      }
      
      // if the current key is the last one → assign the value
      if (index === names.length - 1) {
        temp[name] = current.value;
      }
      
      // reference moves to the next level
      temp = temp[name];
    });
    
    return prev;
  }, {});
};
```

---

## Demo

**Input:**

```html
<form id="parent">
  <input type="text" name="a.c" value="1"/>
  <input type="text" name="a.b.d" value="2"/>
  <input type="text" name="a.b.e" value="3"/>
</form>

<script>
  console.log(aggregateValues('parent'));
</script>
```

**Output:**

```javascript
{
  "a": {
    "c": "1",
    "b": {
      "d": "2",
      "e": "3"
    }
  }
}
```

---

> [!tip] Key Insights
> 
> - Input `name` attributes act as **paths** to construct nested objects.
>     
> - Splitting by `"."` allows dynamic depth handling.
>     
> - Works seamlessly for **dynamic forms** (e.g., survey builders, JSON schema forms).
>     
> - Can be extended to support **arrays** (e.g., `a.b[0].c`) and multiple input types.
>     

---

> [!summary] Takeaway  
> This method is a lightweight polyfill for advanced form libraries.
> 
> - Turns **form fields into structured objects** automatically.
>     
> - Reinforces **DOM manipulation, array reduction, and object traversal**.
>     
> - A practical question for interviews with real-world application in **form state management**.
>     

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/tvvCFeBr1bY)

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/aggregate-the-input-values/)

---
