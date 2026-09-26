

---

> [!quote] Metadata  
> **Posted on:** August 12, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #highlight #string #keywords #frontend #interview

---

# Highlight the Words in a String (JavaScript)

Given a **string** and an **array of keywords**, highlight the words in the string that are part of the keywords array.

---

## Example

```javascript
const str = "Ultimate JavaScript / FrontEnd Guide";
const words = ['Front', 'End', 'JavaScript'];

highlight(str, words);

// "Ultimate <strong>JavaScript</strong> / <strong>FrontEnd</strong> Guide"
```

👉 Notice:  
If two words are **overlapping** or **adjacent**, we **combine them together**.  
In this example, “Front” and “End” are **separate keywords**, but since they appear together as `FrontEnd`, they are **highlighted as one unit**.

---

## Problem Breakdown

To implement this function, we need to handle two different cases:

1. **Direct Match:**
    
    - If a word in the string matches any keyword → highlight it.
        
2. **Partial / Split Match:**
    
    - If no direct match is found:
        
        - Break the word into **two sub-words**.
            
        - If any part matches → highlight only that part.
            
        - If **both parts** match → highlight them **together**.
            

---

## Implementation

```javascript
function highlight(str, keywords) {
  // unique set of keywords
  const uniqueKeywords = new Set(keywords);
  
  // split the str into words
  let words = str.split(" ");
  
  // traverse each word
  const result = words.map(word => {
    // to track the embedding
    let output = '';
    
    // if the word is found in the keywords set
    // highlight it
    if (uniqueKeywords.has(word)) {
      output = `<strong>${word}</strong>`;
    } 
    // else check if the substring of the word is in the keywords set
    else {
      for (let i = 0; i < word.length; i++) {
        // break the word into two parts
        const prefix = word.slice(0, i + 1);
        const suffix = word.slice(i + 1);
         
        // if both the parts are present in keywords
        // embed them together
        if (uniqueKeywords.has(prefix) && uniqueKeywords.has(suffix)) {
          output = `<strong>${prefix}${suffix}</strong>`;
          break;
        } 
        // else if only prefix is present
        else if (uniqueKeywords.has(prefix) && !uniqueKeywords.has(suffix)) {
          output = `<strong>${prefix}</strong>${suffix}`;
        }
        // else if only suffix is present
        else if (!uniqueKeywords.has(prefix) && uniqueKeywords.has(suffix)) {
          output = `${prefix}<strong>${suffix}</strong>`;
        }
      }
    }
    
    // if no embedding has happened, return the original word
    return output !== '' ? output : word;
  });
  
  // form the string back
  return result.join(" ");
}
```

---

## Demo

```javascript
const str = "Ultimate JavaScript / FrontEnd Guide";
const words = ['Front', 'End', 'JavaScript'];

console.log(highlight(str, words));
```

**Output:**

```html
"Ultimate <strong>JavaScript</strong> / <strong>FrontEnd</strong> Guide"
```

---

## Key Observations

- **Direct matches** (e.g., `"JavaScript"`) are highlighted fully.
    
- **Split words** (e.g., `"FrontEnd"`) can be detected by checking prefix/suffix combinations.
    
- **Adjacent keywords** are merged → prevents broken highlighting.
    

---

> [!tip] Insights
> 
> - Using a `Set` ensures **O(1) lookup time** for keywords.
>     
> - Splitting into **prefix/suffix pairs** allows detection of merged keywords.
>     
> - This approach is directly applicable in **search engines, editors, or highlighting matched terms** in UI.
>     

---

> [!summary] Takeaway
> 
> - Highlighting is not just about direct matches.
>     
> - You need to consider **adjacent or overlapping matches**.
>     
> - Efficient keyword lookup (`Set`) + substring handling = robust solution.
>     
> - Useful for building **search highlighting, autocomplete suggestions, or WYSIWYG editors**.
>     

---

### 📎 Reference

- [Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/highlight-the-words-in-the-string/)
    
- 🎥 [YouTube Video Explanation](https://youtu.be/bTMEFFrupyY)
    

---

Do you also want me to **add a section comparing this approach with using Regex** (like `/(${keywords.join('|')})/gi`) so the post has both **manual logic** and **regex-based alternative**?