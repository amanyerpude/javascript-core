
---

> [!quote] Metadata  
> **Posted on:** July 23, 2025  
> **Author:** GeeksforGeeks  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Execute Promises in Series

In JavaScript, executing multiple promises sequentially refers to running asynchronous tasks in a specific order, ensuring each task is completed before the next begins.

---

## Example

```javascript
const promise1 = new Promise((resolve) =>
  setTimeout(() => resolve("Promise 1 resolved"), 1000)
);
const promise2 = new Promise((resolve) =>
  setTimeout(() => resolve("Promise 2 resolved"), 500)
);

function executeSequentially() {
  promise1
    .then((result1) => {
      console.log(result1);
      return promise2;
    })
    .then((result2) => {
      console.log(result2);
    });
}

executeSequentially();
// Output:
// "Promise 1 resolved" (after 1s)
// "Promise 2 resolved" (after 1.5s total)
```

---

## Approach 1: Using Promise.then() Chaining

```javascript
function executeSequentially() {
  promise1
    .then((result1) => {
      console.log(result1);
      return promise2;
    })
    .then((result2) => {
      console.log(result2);
    });
}
```

## Approach 2: Using async/await

```javascript
const fetch1 = () =>
  new Promise((resolve) =>
    setTimeout(() => resolve("Data from promise 1"), 1000)
  );

const fetch2 = () =>
  new Promise((resolve) =>
    setTimeout(() => resolve("Data from promise 2"), 2000)
  );

const fetch3 = () =>
  new Promise((resolve) =>
    setTimeout(() => resolve("Data from promise 3"), 3000)
  );

const executeSequence = async () => {
  const res1 = await fetch1();
  console.log(res1);
  const res2 = await fetch2();
  console.log(res2);
  const res3 = await fetch3();
  console.log(res3);
};

executeSequence();
// Output:
// "Data from promise 1" (after 1s)
// "Data from promise 2" (after 3s)
// "Data from promise 3" (after 6s)
```

---

## Why Execute Promises Sequentially?

- The result of one promise is needed by the next one.
- Operations must happen in a specific order (e.g., fetching data in stages).

---

| Feature | async/await | .then() |
|---------|-------------|---------|
| Syntax | Cleaner and more readable | Can get complex with chains |
| Error Handling | Use try/catch | Uses .catch() |
| Usage | Preferred for newer codebases | Common in older codebases |

---

> [!summary] Takeaway
> - Use **`.then()` chaining** or **async/await** for sequential execution.
> - In `.then()` chains, **return** the next promise to sequence them.
> - With `async/await`, use `await` before each promise call.
> - Sequential execution is slower than parallel but ensures **order**.

---

### 📎 Reference

[How to Execute Multiple Promises Sequentially - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/how-to-execute-multiple-promises-sequentially-in-javascript/)

---
