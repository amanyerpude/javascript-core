
---

> [!quote] Metadata  
> **Posted on:** March 14, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #json #dom #html #interview #frontend

---

# Convert JSON to HTML (JavaScript)

This question was asked in **Meta’s frontend interview**.

👉 **Task:**  
Given a **JSON object** (with `type`, `children`, and `props` representing DOM elements), write a function to **convert the object into actual DOM elements**.

---

## Example

**Input:**

```javascript
const json = { 
  type: 'div', 
  props: { id: 'hello', class: "foo" }, 
  children: [
    { type:'h1', children: 'HELLO' },
    { type:'p', children: [
        { type:'span', props: { class: "bar" }, children: 'World' }
      ] 
    }
  ]
};

JSONtoHTML(json);
```

**Output:**

```html
<div id="hello" class="foo">
  <h1>HELLO</h1>
  <p>
     <span class="bar">World</span>
  </p>
</div>
```

---

## Key Considerations

- The input JSON can be **a single object** or **an array of objects** → must be handled in both cases.
    
- Each JSON node can have:
    
    - `type` → the DOM element type (`div`, `p`, `h1`, `span`, etc.).
        
    - `props` → attributes (like `id`, `class`).
        
    - `children` → can be:
        
        - a string → becomes inner text.
            
        - another object/array → recursive DOM creation.
            

---

## Approach

The logic to implement this function is straightforward:

1. **Create a `DocumentFragment`** to hold the generated DOM nodes.
    
    - Using a fragment prevents multiple reflows/repaints in the DOM.
        
2. **If input is an array:**
    
    - Iterate over each entry.
        
    - For each entry:
        
        - Create the element (`document.createElement`).
            
        - Assign `props` as attributes.
            
        - If `children` is an array → recursively call the same function.
            
        - If `children` is text → set it as `innerText`.
            
    - Append each element into the fragment.
        
3. **If input is a single object:**
    
    - Convert it into an array `[json]` and call recursively.
        
4. **Return the fragment.**
    

---

## Implementation

```javascript
const JSONtoHTML = (json) => {
  // create a fragment
  const fragment = document.createDocumentFragment();
  
  if (Array.isArray(json)) {
    // convert each entry of array to DOM element
    for (let entry of json) {
      // create the element
      const element = document.createElement(entry.type);

      // set attributes if props available
      if (entry.props) {
        for (let key in entry.props) {
          element.setAttribute(key, entry.props[key]);
        }
      }

      // if array of children → recursive call
      if (Array.isArray(entry.children)) {
        for (let child of entry.children) {
          element.appendChild(JSONtoHTML(child));
        }
      }
      // if children is string/text
      else {
        element.innerText = entry.children;
      }

      // add element to fragment
      fragment.appendChild(element);
    }
  } else {
    // if not array → wrap in array and recurse
    return JSONtoHTML([json]);
  }
  
  return fragment;
};
```

---

## Demo with Multiple Root Elements

**Input:**

```javascript
const json = [
  { 
    type: 'div', 
    props: { id: 'hello', class: "foo" }, 
    children: [
      { type:'h1', children: 'HELLO' },
      { type:'p', children: [
          { type:'span', props: { class: "bar" }, children: 'World' }
        ] 
      }
    ]
  },
  { 
    type: 'section', 
    props: { id: 'hello-2', class: "foo-2" }, 
    children: [
      { type:'h1', children: 'HELLO-2' },
      { type:'p', children: [
          { type:'span', props: { class: "bar-2" }, children: 'World' }
        ] 
      }
    ]
  }
];

console.log(JSONtoHTML(json));
```

**Output:**

```html
<div id="hello" class="foo">
  <h1>HELLO</h1>
  <p>
    <span class="bar">World</span>
  </p>
</div>
<section id="hello-2" class="foo-2">
  <h1>HELLO-2</h1>
  <p>
    <span class="bar-2">World</span>
  </p>
</section>
```

---

## Why This Matters in Interviews

- Tests **DOM manipulation fundamentals**.
    
- Requires handling **recursion** properly.
    
- Evaluates ability to deal with **nested structures**.
    
- Simulates real-world tasks like **React’s `createElement`** under the hood.
    

---

> [!tip] Insights
> 
> - `DocumentFragment` is lightweight → avoids unnecessary reflows.
>     
> - Recursive approach mirrors how **virtual DOM libraries (React/Preact)** work internally.
>     
> - Always check if input is **object vs array**.
>     

---

> [!summary] Takeaway
> 
> - Understand how to **map JSON to actual DOM nodes**.
>     
> - Handle both **single objects** and **arrays**.
>     
> - Recursion is the key for **nested children**.
>     
> - This is a **common frontend interview question** (especially at FAANG).
>     

---

### 📎 Reference

- [Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/convert-json-to-html/)
    
- 🎥 [YouTube Video Explanation](https://youtu.be/45CoiSAID2Q)
    

---
