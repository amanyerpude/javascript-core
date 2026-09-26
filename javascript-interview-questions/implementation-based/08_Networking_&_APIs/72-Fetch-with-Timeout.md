
---

> [!quote] Metadata  
> **Posted on:** July 2, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #fetch #timeout

---

# Fetch with Timeout in JavaScript

Create a custom fetch method with Timeout in JavaScript that will terminate the API call if it is not fulfilled in the given duration.

---

## Example

```javascript
fetchWithTimeout('https://jsonplaceholder.typicode.com/todos/1', 100)
  .then((resp) => {
    console.log(resp);
  })
  .catch((error) => {
    console.error(error);
  });

// If response takes longer than 100ms:
// "Aborted"
// error
```

---

## Implementation

The original fetch method does not come with an option to abort in X times. We will make use of `AbortController()` to abort the ongoing network request.

```javascript
const fetchWithTimeout = (url, duration) => {
  return new Promise((resolve, reject) => {
    const controller = new AbortController();
    const signal = controller.signal;
    let timerid = null;

    fetch(url, { signal })
      .then((resp) => {
        resp.json()
          .then((e) => {
            clearTimeout(timerid);
            resolve(e);
          })
          .catch((error) => {
            reject(error);
          });
      })
      .catch((error) => {
        reject(error);
      });

    timerid = setTimeout(() => {
      console.log("Aborted");
      controller.abort();
    }, duration);
  });
};
```

---

## How It Works

1. Create an `AbortController` to manage the abort signal.
2. Start the `fetch` with the abort signal.
3. Simultaneously start a `setTimeout` for the timeout duration.
4. If the fetch completes first, clear the timeout and resolve.
5. If the timeout fires first, abort the fetch and reject.

---

> [!tip] Key Points
> - `AbortController` is the modern way to abort fetch requests.
> - The `signal` property is passed to `fetch` options.
> - `controller.abort()` cancels the in-flight request.
> - Always clean up the timeout with `clearTimeout` on success.

---

> [!summary] Takeaway
> - Wrap `fetch` with `AbortController` for timeout support.
> - Race between fetch completion and timeout.
> - Clean up timeout on successful response.
> - Useful for **slow network** scenarios to prevent hanging.

---

### 📎 Reference

[Fetch with Timeout in JavaScript - LearnersBucket](https://learnersbucket.com/examples/interview/fetch-with-timeout-javascript/)

---
