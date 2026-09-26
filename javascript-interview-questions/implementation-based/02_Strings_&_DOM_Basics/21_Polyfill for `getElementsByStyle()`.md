Perfect 👍 Thanks for sharing the raw content.  
I’ll keep **all the original content exactly as it is**, and just reformat it into the **structured style** (metadata, sections, examples, code blocks, insights, summary, references) like your earlier reformatted posts.  
I’ll also add **extra clarifications** (e.g., why `getComputedStyle` matters, DFS explanation, interview tips) but without removing any of the original details.

Here’s the **final formatted version**:

---

> [!quote] Metadata  
> **Posted on:** February 18, 2025  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #polyfill #style #dom #getComputedStyle #interview

---

# Polyfill for `getElementsByStyle()`

There is **no native method** in JavaScript to get elements by style, which is why we need to create a **polyfill for `getElementsByStyle()`**.

This was asked in a **Zepto SDE3 interview**.  
👉 If you are preparing for frontend interviews, check out [alpha.learnersbucket.com](https://alpha.learnersbucket.com/) where I solve frontend interview questions you can practice.

---

## Why Not Direct Style Matching?

Before jumping into the solution, it’s important to remember:

- Each browser applies styles **differently**.
    
- While there is **standardization**, there’s still unevenness.
    
- To support older browsers, we **should not rely solely** on raw style values.
    

**Example:**

- `color: red`
    
- `color: #ff0000`
    
- `color: rgb(255,0,0)`
    

All three ultimately render the same **computed style** in the browser.

👉 Therefore, to get elements with a certain style reliably, we must always use **`window.getComputedStyle()`**.

---

## Approach

Using `getComputedStyle()`, we can build the polyfill:

1. Apply the style to a **temporary element** → get its computed value.
    
2. Perform a **DFS (Depth First Search)** traversal from the root.
    
3. For each element, compare its computed value of the property.
    
4. If it matches → store that element in the result set.
    

---

## Step 1: Function to Get Computed Value

We first normalize the style property + value to the **browser’s computed value**.

```javascript
const getPropertyComputedValue = (property, value) => {
  // create a new element
  const div = document.createElement('div');

  // apply the property
  div.style[property] = value;

  // get the computed style from DOM
  const styles = window.getComputedStyle(document.body.appendChild(div));

  // read computed value
  let computedValue = styles[property];

  // remove temporary element
  document.body.removeChild(div);

  return computedValue;
}
```

---

## Step 2: Search Elements with Matching Style

Now create the main function:

```javascript
function getElementByStyle(rootElement, property, value) {
  // normalize the property’s computed value
  const computedValue = getPropertyComputedValue(property, value);
  
  const result = [];
  
  // DFS helper
  const search = (element, property, value) => {
    let computedStyles = window.getComputedStyle(element);
    let elementPropertyValue = computedStyles[property];
    
    if (elementPropertyValue === computedValue) {
      result.push(element);
    }
    
    // recursive traversal
    for (const child of element.children) {
      search(child, property, value);
    }
  };
  
  // begin DFS
  search(rootElement, property, value);
  
  return result;
}
```

---

## Test Case

**Input HTML:**

```html
<div id="root">
  <div class="alpha"></div>
  <div class="beta"></div>
  <div class="gamma"></div>
</div>

<style>
#root {
  display: flex;
  gap: 8px;
}

#root > div {
  border: 1px solid;
  width: 50px;
  height: 50px;
}

.alpha {
  padding: 10px 10px;
  background-color: red;
  border-style: dotted !important;
}

.beta {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-right: 10px;
  padding-left: 10px;
  background-color: #000;
}

.gamma {
  padding: 10px 0 0;
  background-color: rgb(255,0,0);
}
</style>

<script>
console.log(getElementByStyle(document.getElementById("root"), 'paddingTop', '10px'));
</script>
```

**Output:**

```javascript
[
  <div class="alpha"></div>,
  <div class="beta"></div>,
  <div class="gamma"></div>
]
```

---

## Why It Works

Even though padding was applied in **different formats** (shorthand, longhand, partial),  
`getComputedStyle()` **normalizes everything** into the same computed value → `"10px"`.

Thus, we get **all elements that effectively have `paddingTop: 10px`**.

---

## Real-World Relevance

This pattern is useful for:

- **Testing** → find elements with applied styles.
    
- **Dynamic DOM manipulation**.
    
- **Browser automation**.
    
- **Polyfill practice** for understanding how native DOM methods might be implemented.
    

---

> [!tip] Insights
> 
> - Always normalize values using **`getComputedStyle()`** before comparison.
>     
> - DFS ensures we **don’t miss nested children**.
>     
> - Works for **shorthand/longhand/hex/rgb** variations.
>     
> - Time complexity: **O(N)** where N = total DOM nodes under root.
>     

---

> [!summary] Takeaway
> 
> - `getElementsByStyle` doesn’t exist natively → but can be polyfilled.
>     
> - Use **`getComputedStyle`** for reliable, browser-consistent results.
>     
> - Implement a **DFS traversal** to check every descendant.
>     
> - A solid interview problem testing **DOM, recursion, and CSS knowledge**.
>     

---

### 📎 Reference

- [Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/polyfill-for-getelementsbystyle/)
    
- 🎥 [YouTube Video Explanation](https://youtu.be/ql3TpOKHiL4)
    

---

Would you like me to also extend this with a **bonus variation**: support for querying **multiple style properties at once** (e.g., `paddingTop=10px` AND `backgroundColor=red`)? That often comes up as an interview follow-up.