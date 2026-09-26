
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** LearnersBucket  
> **Posted in:** Interview, JavaScript, React  
> **Tags:** #interview #javascript #react #meta #polyfill

---

# Polyfill for ClassNames in React

Write a polyfill for Classnames, which is the most popular package to dynamically add classes to JSX elements in React.

---

## Example

```javascript
classnames('foo', 'bar'); // => 'foo bar'
classnames('foo', { bar: true }); // => 'foo bar'
classnames({ 'foo-bar': true }); // => 'foo-bar'
classnames({ 'foo-bar': false }); // => ''
classnames('foo', { bar: true, baz: false }); // => 'foo bar'
classnames('foo', ['bar', 'baz']); // => 'foo bar baz'
```

---

## Implementation

```javascript
function classNames(...args) {
  const classes = [];

  for (const arg of args) {
    if (!arg) continue;

    const type = typeof arg;

    if (type === 'string' || type === 'number') {
      classes.push(arg);
    } else if (Array.isArray(arg)) {
      const inner = classNames(...arg);
      if (inner) {
        classes.push(inner);
      }
    } else if (type === 'object') {
      for (const key in arg) {
        if (arg.hasOwnProperty(key) && arg[key]) {
          classes.push(key);
        }
      }
    }
  }

  return classes.join(' ');
}
```

---

## How It Works

1. **Strings/Numbers**: Add directly to the classes array.
2. **Arrays**: Recursively process nested arrays.
3. **Objects**: Add keys where value is truthy.
4. **Falsy values** (null, undefined, false): Skip entirely.
5. **Join** all collected classes with a space.

---

> [!tip] Key Points
> - Handles **strings**, **objects**, **arrays**, and **nested** combinations.
> - Truthy object keys are added as class names.
> - Falsy values are **filtered out**.
> - Similar to the `classnames` npm package by Jed Watson.

---

> [!summary] Takeaway
> - `classNames` dynamically builds class strings from mixed inputs.
> - Supports **strings**, **objects** (truthy keys), and **arrays**.
> - Falsy values are **ignored**.
> - Useful for **conditional class application** in React components.

---

### 📎 Reference

[Polyfills for ClassNames in React - YouTube (LearnersBucket)](https://www.youtube.com/watch?v=GZ0uRKjPqFQ)

---
