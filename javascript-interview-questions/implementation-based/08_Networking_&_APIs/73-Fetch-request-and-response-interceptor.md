
---

> [!quote] Metadata  
> **Posted on:** January 3, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #fetch #interceptor

---

# Fetch Request and Response Interceptor

Add a request and response interceptor method to fetch that can be used to monitor each request and response.

---

## Example

```javascript
const requestInterceptor = (requestArguments) => {
  console.log("Before request");
};

const responseInterceptor = (response) => {
  console.log("After response");
};

fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then((response) => response.json())
  .then((json) => console.log(json));

// "Before request"
// "After response"
```

---

## Implementation

```javascript
// store the original fetch
const originalFetch = window.fetch;

// request interceptor
window.requestInterceptor = (args) => {
  // your action goes here
  return args;
};

// response interceptor
window.responseInterceptor = (response) => {
  // your actions goes here
  return response;
};

// over-ride the original fetch
window.fetch = async (...args) => {
  // request interceptor
  args = requestInterceptor(args);

  // pass the updated args to fetch
  let response = await originalFetch(...args);

  // response interceptor
  response = responseInterceptor(response);

  // return the updated response
  return response;
};
```

---

## Test Case

```javascript
// request interceptor - add pagination
window.requestInterceptor = (args) => {
  args[0] = args[0] + "2";
  return args;
};

// response interceptor - parse JSON
window.responseInterceptor = (response) => {
  return response.json();
};

fetch('https://jsonplaceholder.typicode.com/todos/')
  .then((json) => console.log(json));

// Output:
// { "userId": 1, "id": 2, "title": "quis ut nam facilis et officia qui", "completed": false }
```

---

> [!tip] Key Points
> - Override `window.fetch` with a custom implementation.
> - Store the **original fetch** before overriding.
> - Request interceptor modifies **arguments** before the call.
> - Response interceptor modifies **response** after the call.
> - Similar to how **Axios** interceptors work.

---

> [!summary] Takeaway
> - Override `window.fetch` to add interceptor support.
> - Request interceptor runs **before** each fetch call.
> - Response interceptor runs **after** each fetch response.
> - Useful for **logging**, **auth headers**, **error handling**.

---

### 📎 Reference

[Fetch request and response interceptor - LearnersBucket](https://learnersbucket.com/examples/interview/fetch-request-and-response-interceptor/)

---
