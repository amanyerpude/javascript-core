
---

> [!quote] Metadata  
> **Posted on:** April 15, 2024  
> **Author:** GeeksforGeeks  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #currying #placeholder

---

# Currying with Placeholder Support

Currying with placeholder support is an extension of traditional currying that allows placeholders to be used for arguments. Placeholders are special markers that indicate where arguments should be supplied when invoking a curried function.

---

## Example

```javascript
const concat3 = curry((a, b, c) => `${a} ${b} ${c}`);

// Partial application using placeholders
const concatHello = concat3('Hello,', curry.placeholder, 'World!');

console.log(concatHello('Welcome'));    // "Hello, Welcome World!"
console.log(concatHello('Greetings'));  // "Hello, Greetings World!"
```

---

## Implementation

```javascript
const curry = (fn) => {
  return function curried(...args) {
    // If enough arguments are provided
    // and no placeholders remain, call the function
    if (args.length >= fn.length && !args.includes(curry.placeholder)) {
      return fn.apply(this, args);
    } else {
      // Otherwise, return a curried function
      // with placeholder support
      return function (...nextArgs) {
        const combinedArgs = args.map(
          (arg) =>
            arg === curry.placeholder && nextArgs.length
              ? nextArgs.shift()
              : arg
        ).concat(nextArgs);
        return curried(...combinedArgs);
      };
    }
  };
};

// Placeholder symbol for missing arguments
curry.placeholder = Symbol();
```

---

## How It Works

1. Check if enough arguments are provided **and** no placeholders remain.
2. If yes, **call** the original function.
3. If no, return a **new curried function**.
4. When the new function is called, **replace** placeholders with actual values.
5. Use `Symbol()` for the placeholder to avoid collisions.

---

## Difference from Traditional Currying

| Feature | Currying | Currying with Placeholder |
|---------|----------|--------------------------|
| Placeholders | No | Yes |
| Argument order | Fixed | Flexible |
| Partial application | Sequential | Any position |

---

> [!tip] Key Points
> - Use `Symbol()` for placeholder to avoid value collisions.
> - `map` replaces placeholders with next available arguments.
> - `concat(nextArgs)` appends remaining arguments.
> - Enables **flexible partial application** in any order.

---

> [!summary] Takeaway
> - Currying with placeholders allows **flexible argument positioning**.
> - Placeholders reserve spots for later arguments.
> - Use `Symbol()` for unique placeholder identity.
> - Enables more **expressive** and **composable** function APIs.

---

### 📎 Reference

[Currying with Placeholder Support - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/currying-with-placeholder-support-in-javascript/)

---
