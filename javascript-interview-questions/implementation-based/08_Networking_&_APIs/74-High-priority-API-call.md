
---

> [!quote] Metadata  
> **Posted on:** December 16, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #fetch #priority

---

# Make High Priority API Call in JavaScript

While you're throttling your requests, suppose a high-priority API call needs to be made, how would you manage it?

---

## Approach 1: Using Request.Priority

The fetch method comes with an additional option to prioritize the API requests.

```javascript
// articles list (high by default)
let articles = await fetch('/articles');

// articles recommendation list (suggested low)
let recommendation = await fetch('/articles/recommendation', {
  priority: 'low'
});
```

Priority values: `low`, `high`, `auto` (default is `high`).

---

## Approach 2: Using Microtask Queue

While throttling, calls are made after some delay using timer functions. We can make a high-priority call between the consecutive timer functions using `queueMicrotask`.

```javascript
let callback = () => {
  fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then((response) => response.json())
    .then((json) => console.log(json));
};

let callback2 = () => {
  fetch('https://jsonplaceholder.typicode.com/todos/2')
    .then((response) => response.json())
    .then((json) => console.log(json));
};

let urgentCallback = () => {
  fetch('https://jsonplaceholder.typicode.com/todos/3')
    .then((response) => response.json())
    .then((json) => console.log(json));
};

console.log("Main program started");
setTimeout(callback, 0);
setTimeout(callback2, 10);
queueMicrotask(urgentCallback);
console.log("Main program exiting");

// Output:
// "Main program started"
// "Main program exiting"
// { id: 3, ... }    ← microtask runs before timers
// { id: 1, ... }
// { id: 2, ... }
```

---

> [!tip] Key Points
> - `queueMicrotask` runs before `setTimeout` callbacks.
> - Microtasks execute after the current task but before macrotasks.
> - Use `Request.priority` for native browser prioritization.
> - Note: calls are made in priority, but resolution time varies.

---

> [!summary] Takeaway
> - Use `Request.priority` for browser-native prioritization.
> - Use `queueMicrotask` to execute urgent calls before timer-based ones.
> - Microtasks run between the current task and the next macrotask.
> - Useful when you need to **bypass** throttle for critical requests.

---

### 📎 Reference

[Make high priority Api call - LearnersBucket](https://learnersbucket.com/examples/interview/make-high-priority-api-call-in-javascript/)

---
