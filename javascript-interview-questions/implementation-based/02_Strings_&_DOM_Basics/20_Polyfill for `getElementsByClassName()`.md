Perfect 👍 Thanks for passing the draft.  
I’ll **not remove any content**, I’ll keep it all intact while reformatting into the **structured style** (metadata, examples, code, explanations, insights, summary, references).  
I’ll also add **extra clarifications** (like DFS, interview notes, alternatives) but without cutting the original content.

Here’s the **final formatted version**:

---

> [!quote] Metadata  
> **Posted on:** October 15, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #polyfill #dom #class #dfs #interview

---

# Polyfill for `getElementsByClassName()`

**Task:**  
Write a custom function to find all the elements with the given class in the DOM.  
In other words, create a **polyfill for `getElementsByClassName()`**.

---

## Example

**Input:**

```html
<div class='a' id="root">
  <div class='b' id='b-1'>
    <div class='a' id='a-2'>
      <div class='d' id='d-1'></div>
    </div>
    <div class='c' id='c-1'>
      <div class='a' id='a-3'>
        <div class='d' id='d-2'></div>
      </div>
    </div>
  </div>
</div>

<script>
  findByClass('a');
</script>
```

**Output:**

```javascript
[
  <div class="a" id="root">...</div>,
  <div class="a" id="a-2">...</div>,
  <div class="a" id="a-3">...</div>
]
```

---

## Approach

The solution is quite straightforward:

1. A DOM element can have **multiple classes**.
    
2. Start from the **root node** (or `document.body`).
    
3. Check if the node’s `classList` contains the desired class → if yes, add it to the result.
    
4. Then **traverse all the children** of the current node.
    
5. For each child, recursively apply the same logic.
    

This traversal pattern is known as **DFS (Depth First Search)**.

---

## Implementation

```javascript
function findByClass(className) {
  // get the root element (you can start from body or any specific node)
  const root = document.body;
  
  // helper function to perform DFS search
  function search(node) {
    let result = [];
    
    // if class name is present in the node's classList
    if (node.classList.contains(className)) {
       result.push(node);
    }
   
    // recursively search in children
    for (const element of node.children) {
      const res = search(element);
      result = result.concat(res);
    }
    
    return result;
  }
  
  // initiate DFS and return result
  return search(root);
}
```

---

## Demo

**Input:**

```html
<div class='a' id="root">
  <div class='b' id='b-1'>
    <div class='a' id='a-2'>
      <div class='d' id='d-1'></div>
    </div>
    <div class='c' id='c-1'>
      <div class='a' id='a-3'>
        <div class='d' id='d-2'></div>
      </div>
    </div>
  </div>
</div>

<script>
  console.log(findByClass('a'));
</script>
```

**Output:**

```javascript
[
  <div class="a" id="root">...</div>,
  <div class="a" id="a-2">...</div>,
  <div class="a" id="a-3">...</div>
]
```

---

## Why DFS Works Best

- **Depth-first traversal** ensures we check **every descendant** of the root.
    
- Perfect for recursive problems like DOM tree traversal.
    
- Matches how browsers internally walk the DOM tree for queries.
    

---

## Interview Notes

- This polyfill checks **`classList.contains()`** → the most reliable way to match classes.
    
- Be careful: a node might have **multiple classes** (`class="a b c"`) → so direct string equality won’t work.
    
- Time complexity: **O(N)** where N = total DOM nodes.
    
- Similar logic can be adapted to polyfill:
    
    - `getElementsByTagName()`
        
    - `getElementById()`
        

---

> [!tip] Insights
> 
> - DOM is a **tree**, so recursion fits naturally.
>     
> - You can optimize by passing a different **starting root node** (instead of `document.body`).
>     
> - Using **BFS (queue-based traversal)** is also possible, but DFS recursion is simpler for interviews.
>     

---

> [!summary] Takeaway
> 
> - Polyfill for `getElementsByClassName` is a **classic interview question**.
>     
> - Tests **recursion, DOM traversal, and class handling**.
>     
> - Key concept: implement a **DFS search** and check `classList.contains()`.
>     

---

### 📎 Reference

- [Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/polyfill-for-getelementbyclassname/)
    
- 🎥 [YouTube Video Explanation](https://youtu.be/3b35wf3CLeQ)
    

---

Do you want me to also extend this with a **bonus variation** → “What if we wanted to support multiple classes (e.g., find elements containing both `a` and `b`)?” That’s a common follow-up in interviews.