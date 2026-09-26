
---

> [!quote] Metadata  
> **Posted on:** June 2, 2023  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #function #cookie #document

---

# Create Custom Cookie in JavaScript

Create your own custom cookie available on the `document` object with support for the **`max-age`** option.

---

## Problem Statement

- Extend the `document` object with a property `myCookie`.
    
- Support setting cookies with an optional **`max-age`** attribute.
    
- When retrieving, expired cookies should be automatically removed.
    
- Return cookies as a string of `key=value` pairs separated by `;` .
    

---

> [!example] Example
> 
> ```javascript
> useCustomCookie();
> 
> document.myCookie = "blog=learnersbucket";
> document.myCookie = "name=prashant;max-age=1"; // expires after 1 second
> 
> console.log(document.myCookie);
> // "blog=learnersbucket; name=prashant"
> 
> setTimeout(() => {
>   console.log(document.myCookie);
> }, 1500);
> // After 1.5 seconds → "blog=learnersbucket"
> ```

---

## Concept

- Use **`Object.defineProperty()`** to extend `document` with `myCookie`.
    
- Behind the scenes:
    
    - **Setter:** Parses the cookie string, extracts `key`, `value`, and options (like `max-age`), then stores them in a `Map`.
        
    - **Getter:** Iterates over stored cookies, removes expired ones, and returns the remaining cookies as a formatted string.
        
- Helper functions are used to parse input strings and separate keys, values, and options.
    

---

## Helper Functions

```javascript
// Parse "name=value; option=value"
function parseCookieString(str) {
  const [nameValue, ...rest] = str.split(';');
  const [key, value] = separateKeyValue(nameValue);

  const options = {};
  for (const option of rest) {
    const [k, v] = separateKeyValue(option);
    options[k] = v;
  }

  return { key, value, options };
}

// Split key and value
function separateKeyValue(str) {
  return str.split('=').map(s => s.trim());
}
```

---

## Main Implementation

```javascript
// Enable custom cookie property
function useCustomCookie() {
  const store = new Map();

  Object.defineProperty(document, 'myCookie', {
    configurable: true,

    get() {
      const cookies = [];
      const time = Date.now();

      for (const [name, { value, expires }] of store) {
        if (expires <= time) {
          store.delete(name); // remove expired
        } else {
          cookies.push(`${name}=${value}`);
        }
      }

      return cookies.join('; ');
    },

    set(val) {
      const { key, value, options } = parseCookieString(val);

      let expires = Infinity;
      if (options["max-age"]) {
        expires = Date.now() + Number(options["max-age"]) * 1000;
      }

      store.set(key, { value, expires });
    }
  });
}
```

---

## Input / Output

```javascript
useCustomCookie();
document.myCookie = "blog=learnersbucket";

// expires after 1 second
document.myCookie = "name=prashant;max-age=1"; 

console.log(document.myCookie);
// "blog=learnersbucket; name=prashant"

setTimeout(() => {
  console.log(document.myCookie);
}, 1500);
// "blog=learnersbucket"
```

---

> [!tip] Key Insights
> 
> - Cookies are stored in a **Map** for easy key/value management.
>     
> - Expired cookies are **automatically removed** when accessed.
>     
> - This is a **custom implementation** for practice and interview scenarios — not a replacement for browser-native cookies.
>     

---

> [!summary] Takeaway  
> By extending `document` with `Object.defineProperty()`, we can simulate cookie behavior with expiration (`max-age`) while learning about **property descriptors**, **closures**, and **state management** in JavaScript.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/21rvzoieIU0)

---
