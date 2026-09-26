

---

> [!quote] Metadata  
> **Posted on:** August 14, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #html #encoding #string #interview #wysiwyg

---

# HTML Encoding of a String in JavaScript

Given a **string** and an **array representing the HTML encoding** of the string (`from`, `to`, with a given tag).  
The task is to **return the HTML encoded string**.

---

## Example

**Input:**

```javascript
const str = 'Hello, world'; 
const styleArr = [[0, 2, 'i'], [4, 9, 'b'], [7, 10, 'u']];
```

**Output:**

```html
<i>Hel</i>l<b>o, w<u>orl</u></b><u>d</u>
```

> 📝 **Note:**  
> The `<u>` tag gets placed **before the `<b>` tag and after it** as the insertion index overlaps.

---

## Why is this Important?

This feature is one of the **most common implementations** in **WYSIWYG editors**:

- You type normal text.
    
- Apply styles (italic, bold, underline).
    
- The editor **converts it into HTML encoding** behind the scenes.
    

---

## Two Approaches

There are two ways to solve this problem:

1. **Using an inbuilt method** (simpler, DOMParser-based).
    
2. **Custom algorithm** (more complex, often asked in interviews).
    

Since interviews usually expect the **custom implementation**, let’s start with that.

---

## Custom Algorithm for HTML Encoding

The main challenge: **styles can overlap**.  
This means one tag can partially overlap another → requiring us to **close the overlapping tag and re-open it again**.

---

### Example of Overlap

**Input:**

```javascript
const str = "Hello, World";
const style =  [0, 2, 'i'], [1, 3, 'b'];
```

**Output:**

```html
<i>H<b>el</b></i><b>l</b>o, World
```

👉 Here, `<b>` starts at index 1 and ends at 3, overlapping `<i>`.  
We therefore **close `<i>` at index 2** and **re-open it later**.

---

### Data Structures Used

To implement this robustly, we use:

- **Priority Queue** → ensures longest tags are handled first.
    
- **Stack** → manages nested tags and their order.
    

---

### Step 1: Priority Queue Helper

```javascript
function addAndSort(track, index, data) {
  if (!track[index]) track[index] = [];
  track[index] = [...track[index], data];
  track[index].sort((a, b) => a.getRange() > b.getRange());
};
```

> [!tip] Why a Priority Queue?  
> It ensures tags with **larger ranges** get processed first.  
> Without this, overlapping tags can get incorrectly nested.

---

### Step 2: Stack Implementation

```javascript
function Stack() {
  let items = [];
  let top = 0;

  this.push = function (element) {
    items[top++] = element;
  };

  this.pop = function () {
    return items[--top];
  };

  this.peek = function () {
    return items[top - 1];
  };
};
```

---

### Step 3: Tag Object

```javascript
function Tag(start, end, tag) {
  this.start = start;
  this.end = end;
  this.tag = tag;
  this.text = "";

  this.getRange = () => {
    return this.end - this.start;
  };
};
```

---

### Step 4: Encoder Method

```javascript
function parse(str, markups) {
  const track = new Array(str.length).fill(null);

  for (let markup of markups) {
    const [start, end, tag] = markup;
    addAndSort(track, start, new Tag(start, end, tag));
  }

  const html = new Stack();
  html.push(new Tag(0, Number.MAX_VALUE, ""));

  for (let i = 0; i < str.length; i++) {
    while (track[i] && track[i].length > 0) {
      const cur = track[i].shift();
      cur.text = `<${cur.tag}>`;

      if (cur.end > html.peek().end) {
        const split = new Tag(html.peek().end + 1, cur.end, cur.tag);
        cur.end = html.peek().end;
        addAndSort(track, html.peek().end + 1, split);
      }
      html.push(cur);
    }

    html.peek().text += str[i];

    while (html.peek().end === i) {
      html.peek().text += `</${html.peek().tag}>`;
      const temp = html.pop().text;
      html.peek().text += temp;
    }
  }
  return html.pop().text;
};
```

---

### Demo

```javascript
const encoded = parse('Hello, World',
[
  [0, 2, "i"],
  [7, 10, "u"],
  [4, 9, "b"],
  [2, 7, "i"],
  [7, 9, "u"]
]); 

console.log(encoded);
```

**Output:**

```html
<i>He<i>l</i></i><i>l<b>o, <u><u>W</u></u></b></i><b><u><u>or</u></u></b><u>l</u>d
```

---

## Alternative: Using `DOMParser()`

The **simpler approach** uses the built-in `DOMParser()`.  
It automatically **places missing opening/closing tags correctly**.

---

### Implementation

```javascript
function parse(string, markups) {
  const fragments = markups.reduce((chars, [start, end, tag]) => {
    chars[start] = `<${tag}>` + chars[start];
    chars[end] += `</${tag}>`;
    return chars;
  }, [...string]);

  return new DOMParser()
      .parseFromString(fragments.join(''), 'text/html')
      .body.innerHTML;
}
```

---

### Demo

```javascript
const encoded = parse('Hello, World',
[
  [0, 2, "i"],
  [7, 10, "u"],
  [4, 9, "b"],
  [2, 7, "i"],
  [7, 9, "u"]
]); 

console.log(encoded);
```

**Output:**

```html
<i>He<i>l</i>l<b>o, <u><u>W</u></u></b></i><b><u><u>or</u></u></b><u>l</u>d
```

---

> [!tip] Key Insights
> 
> - **Custom algorithm** → great for interviews, teaches data structures (stack, priority queue).
>     
> - **DOMParser approach** → great for production, simple and robust.
>     
> - **Overlapping tags** are the real challenge in this problem.
>     
> - This is exactly how **text editors handle formatting** under the hood.
>     

---

> [!summary] Takeaway  
> Implementing HTML encoding of a string helps you understand:
> 
> - Managing **overlapping styles**.
>     
> - Applying **data structures** (stack + priority queue).
>     
> - Difference between **manual vs built-in** approaches.
>     
> - Real-world relevance in **WYSIWYG editors**.
>     

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/html-encoding-of-a-string/)

---
