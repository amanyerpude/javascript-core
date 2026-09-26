Got it 👍 Thanks for the detailed draft.  
I’ll **not remove any of your original content**, I’ll keep everything intact while reformatting it into the **structured style** (metadata, sections, examples, code, explanation, callouts, insights, summary, references).  
I’ll also add **extra clarifications** (like recursion flow, interview notes, edge cases) but won’t reduce the post in any way.

Here’s the **final formatted version**:

---

> [!quote] Metadata  
> **Posted on:** April 4, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #polyfill #dom #class #hierarchy #interview

---

# Implement `getByClassNameHierarchy()` Function in JavaScript

**Task:**  
Write a function `getByClassNameHierarchy()` in JavaScript that takes a **path of class names** and returns the **last element(s)** of that path.

---

## Example

**Input:**

```html
<div class="a" id="a-1">
  <div class="b" id="b-1">
    <div class="c" id="c-1"/>
    <div class="c" id="c-2"/>
  </div>
  <div class="c" id="c-3"/>
</div>

<script>
getByClassNameHierarchy("a>b>c");
</script>
```

**Output:**

```javascript
[
  <div class="c" id="c-1"></div>, 
  <div class="c" id="c-2"></div>
]
```

---

## Background

This function is like an **extended version of `getElementsByClassName()`**.  
Instead of finding elements by a **single class**, we find elements that appear in a **class hierarchy (path)**.

👉 Example: `"a>b>c"` means → find all `c` elements that are **inside** a `b` element which is **inside** an `a` element.

---

## Approach

We can solve this using the following steps:

1. **Split the input path** → Convert `"a>b>c"` into `['a','b','c']`.
    
2. **Use recursion with an index tracker**:
    
    - At each step, check if the current node has the class.
        
    - If it’s the **last class in the path** and matches → add it to results.
        
    - Otherwise, traverse its children.
        
3. **DFS traversal** → Recursively search down the DOM tree.
    
4. If a child doesn’t match the current class, restart the search from the **first class in the hierarchy**.
    

---

## Implementation

```javascript
function getByClassNameHierarchy(element, classNames) {
  // split path into class array
  const classList = classNames.split('>');
  
  const result = [];
  
  // recursive traversal
  traverseDOM(element, classList, 0, result);
  
  return result;
}

// helper function
function traverseDOM(element, classNames, index, result) {
  // base case: null element
  if (!element) return;
  
  const targetClass = classNames[index];
   
  // if last class in hierarchy & matches → add to results
  if (index === classNames.length - 1 && element.classList.contains(targetClass)) {
    result.push(element);
    return;
  }
  
  // traverse children
  for (const child of element.children) {
    if (element.classList.contains(targetClass)) {
      // move to next class
      traverseDOM(child, classNames, index + 1, result);
    } else {
      // restart search from beginning
      traverseDOM(child, classNames, 0, result);
    }
  }
}
```

---

## Demo

**Input:**

```html
<div class="a" id="root">
  <div class="b" id="b-1">
    <div class="c" id="c-1"></div>
    <div class="c" id="c-2"></div>
  </div>
  <div class="b" id="b-2">
    <div class="c" id="c-3"></div>
  </div>
</div>

<script>
console.log(getByClassNameHierarchy(document.getElementById('root'), 'a>b>c'));
</script>
```

**Output:**

```javascript
[
  <div class="c" id="c-1"></div>,
  <div class="c" id="c-2"></div>,
  <div class="c" id="c-3"></div>
]
```

---

## How It Works

- **Index tracking**: Keeps track of where we are in the class hierarchy.
    
- **Recursion**: DFS is used to search all children.
    
- **Path reset**: If a node doesn’t match the expected class, restart from index `0`.
    
- **Final match**: Only when we reach the last class (`c` in `"a>b>c"`) and it matches, do we add to results.
    

---

## Edge Cases

- If **no matching path** exists → returns an empty array.
    
- If multiple matching paths exist → returns all of them.
    
- Works for **deeply nested hierarchies**.
    

---

## Interview Relevance

This question tests:

- **DOM traversal skills**.
    
- **Recursive problem solving**.
    
- Understanding of **hierarchical selectors** (similar to CSS `a > b > c`).
    
- Ability to handle **edge cases** when hierarchy doesn’t match.
    

---

> [!tip] Insights
> 
> - Works similar to how **CSS child selectors** operate.
>     
> - DFS is a natural fit for DOM traversal.
>     
> - Restarting the path ensures you don’t miss valid matches deeper in the tree.
>     

---

> [!summary] Takeaway
> 
> - Extend `getElementsByClassName` to support **hierarchical paths**.
>     
> - Split input string (`a>b>c`) into array and track progress with recursion.
>     
> - Add elements only when **full hierarchy is satisfied**.
>     
> - A solid interview problem combining **DOM + recursion + CSS-like logic**.
>     

---

### 📎 Reference

- [Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/implement-getbyclassnamehierarchy-function-in-javascript/)
    
- 🎥 [YouTube Video Explanation](https://youtu.be/1p6Ow7qIH_k)
    

---

Do you also want me to add a **bonus variation** → supporting `"a b c"` (descendant selectors) instead of only `>` (direct child selectors), since that’s a common CSS-like extension?