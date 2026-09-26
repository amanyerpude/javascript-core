
---

> [!quote] Metadata  
> **Posted on:** May 2, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #promises

---

# Retry Promises N Number of Times

Implement a function in JavaScript that retries promises N number of times with a delay between each call.

---

## Example

```javascript
retry(asyncFn, retries = 3, delay = 50, finalError = 'Failed');

// Output:
// ... attempt 1 -> failed
// ... attempt 2 -> retry after 50ms -> failed
// ... attempt 3 -> retry after 50ms -> failed
// ... Failed.
```

---

## Using then...catch

```javascript
// delay function
const wait = (ms) =>
  new Promise((resolve) => {
    setTimeout(() => resolve(), ms);
  });

const retryWithDelay = (
  operation,
  retries = 3,
  delay = 50,
  finalErr = 'Retry failed'
) =>
  new Promise((resolve, reject) => {
    return operation()
      .then(resolve)
      .catch((reason) => {
        // if retries are left
        if (retries > 0) {
          // delay the next call
          return wait(delay)
            // recursively call with max retries - 1
            .then(
              retryWithDelay.bind(
                null,
                operation,
                retries - 1,
                delay,
                finalErr
              )
            )
            .then(resolve)
            .catch(reject);
        }
        // throw final error
        return reject(finalErr);
      });
  });
```

---

## Using async...await

```javascript
const retryWithDelay = async (
  fn,
  retries = 3,
  interval = 50,
  finalErr = 'Retry failed'
) => {
  try {
    await fn();
  } catch (err) {
    // if no retries left, throw error
    if (retries <= 0) {
      return Promise.reject(finalErr);
    }
    // delay the next call
    await wait(interval);
    // recursively call the same func
    return retryWithDelay(fn, retries - 1, interval, finalErr);
  }
};
```

---

## Test Case

```javascript
const getTestFunc = () => {
  let callCounter = 0;
  return async () => {
    callCounter += 1;
    if (callCounter < 5) {
      throw new Error('Not yet');
    }
  };
};

const test = async () => {
  await retryWithDelay(getTestFunc(), 10);
  console.log('success');

  await retryWithDelay(getTestFunc(), 3);
  console.log('will fail before getting here');
};

test().catch(console.error);
// Output:
// "success"        // 1st test (retries enough times)
// "Retry failed"   // 2nd test (not enough retries)
```

---

> [!tip] Key Points
> - Use **recursion** to retry the same function with reduced retries.
> - Add a **delay** between retries using `setTimeout` / `Promise`.
> - Track retries count — when it hits 0, reject with the final error.
> - Both `then...catch` and `async...await` approaches work.

---

> [!summary] Takeaway
> - Retry pattern uses **recursive** calls with decremented retry count.
> - Add **delay** between retries to avoid hammering the server.
> - When retries are exhausted, **reject** with a final error message.
> - Common in **network requests** that may fail intermittently.

---

### 📎 Reference

[Retry promises N number of times - LearnersBucket](https://learnersbucket.com/examples/interview/retry-promises-n-number-of-times-in-javascript/)

---
