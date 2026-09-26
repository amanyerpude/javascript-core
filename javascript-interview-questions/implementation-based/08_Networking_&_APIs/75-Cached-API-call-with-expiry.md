
---

> [!quote] Metadata  
> **Posted on:** June 28, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #cache

---

# Cached API Call with Expiry Time

Implement a function in JavaScript that caches the API response for the given amount of time. If a new call is made between that time, the response from the cache will be returned, else a fresh API call will be made.

---

## Example

```javascript
const call = cachedApiCall(1500);

// first call — API call will be made and cached
call('https://jsonplaceholder.typicode.com/todos/1', {}).then((a) => console.log(a));
// "making new api call"

// cached response — quick
setTimeout(() => {
  call('https://jsonplaceholder.typicode.com/todos/1', {}).then((a) => console.log(a));
}, 700);

// fresh API call — cache expired
setTimeout(() => {
  call('https://jsonplaceholder.typicode.com/todos/1', {}).then((a) => console.log(a));
}, 2000);
// "making new api call"
```

---

## Helper Functions

```javascript
// generate unique key from input
const generateKey = (path, config) => {
  const key = Object.keys(config)
    .sort((a, b) => a.localeCompare(b))
    .map((k) => k + ":" + config[k].toString())
    .join("&");
  return path + key;
};

// make API call
const makeApiCall = async (path, config) => {
  try {
    let response = await fetch(path, config);
    response = await response.json();
    return response;
  } catch (e) {
    console.log("error " + e);
  }
  return null;
};
```

---

## Main Function

```javascript
const cachedApiCall = (time) => {
  const cache = {};

  return async function (path, config = {}) {
    const key = generateKey(path, config);
    let entry = cache[key];

    // if no cached data or expired
    if (!entry || Date.now() > entry.expiryTime) {
      console.log("making new api call");
      try {
        const value = await makeApiCall(path, config);
        cache[key] = { value, expiryTime: Date.now() + time };
      } catch (e) {
        console.log(e);
      }
    }

    return cache[key].value;
  };
};
```

---

## How It Works

1. **Closure** stores the cache object and expiry time.
2. Generate a **unique key** from URL + config.
3. Check if cache entry **exists** and is **not expired**.
4. If expired or missing, make a **fresh API call** and cache the result.
5. Return the **cached** or **fresh** result.

---

> [!tip] Key Points
> - Uses **closure** to maintain cache state across calls.
> - Generate unique keys from **URL + sorted config**.
> - Compare `Date.now()` with `expiryTime` to check freshness.
> - Cache entries include both the **value** and **expiry timestamp**.

---

> [!summary] Takeaway
> - Cache API responses with **time-based expiry**.
> - Use **closure** to maintain cache state.
> - Generate **unique keys** from request parameters.
> - Return cached data for repeated calls within the expiry window.

---

### 📎 Reference

[Cached api call with expiry time - LearnersBucket](https://learnersbucket.com/examples/interview/cached-api-call-with-expiry-time/)

---
