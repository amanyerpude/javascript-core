
---

> [!quote] Metadata  
> **Posted on:** June 25, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, React  
> **Tags:** #interview #react #rippling #race-condition

---

# Handle Race Condition in React

A race condition is a phenomenon in which if you are making multiple API calls or performing asynchronous operations, then there are chances that the UI can update/render in glitch as the later call may resolve first and the first API call may resolve later.

---

## The Problem

```javascript
const App = (props) => {
  const [data, setData] = useState({});

  useEffect(() => {
    const fetchData = async () => {
      let resp = await fetch(`https://jsonplaceholder.typicode.com/todos/${props.id}`);
      resp = await resp.json();
      setData(resp);
    };
    fetchData();
  }, [props.id]);

  return <div>{data.title || "Hello World!"}</div>;
};
```

If the component receives multiple ids rapidly, the UI may render data from the **wrong** API call.

---

## Fix 1: Using Flag in useEffect

```javascript
const App = (props) => {
  const [data, setData] = useState({});

  useEffect(() => {
    let flag = true;

    const fetchData = async () => {
      let resp = await fetch(`https://jsonplaceholder.typicode.com/todos/${props.id}`);
      resp = await resp.json();
      if (flag) {
        setData(resp);
      }
    };

    fetchData();

    return () => {
      flag = false;
    };
  }, [props.id]);

  return <div>{data.title || "Hello World!"}</div>;
};
```

---

## Fix 2: Using AbortController

```javascript
const App = (props) => {
  const [data, setData] = useState({});

  useEffect(() => {
    const abortController = new AbortController();

    const fetchData = async () => {
      try {
        let resp = await fetch(
          `https://jsonplaceholder.typicode.com/todos/${props.id}`,
          { signal: abortController.signal }
        );
        resp = await resp.json();
        setData(resp);
      } catch (error) {
        // abort controller throws error when aborted
      }
    };

    fetchData();

    return () => {
      abortController.abort();
    };
  }, [props.id]);

  return <div>{data.title || "Hello World!"}</div>;
};
```

---

> [!tip] Key Points
> - **Flag approach**: Set flag to false on cleanup to prevent stale updates.
> - **AbortController**: Cancel the fetch request on cleanup.
> - AbortController only works with **fetch**; use flag for other async ops.
> - Always handle the **abort error** in catch block.

---

> [!summary] Takeaway
> - Race conditions occur when **multiple async operations** overlap.
> - Fix with a **cleanup flag** in useEffect.
> - Fix with **AbortController** to cancel in-flight requests.
> - Always **clean up** side effects in useEffect return function.

---

### 📎 Reference

[Handle race condition in React - LearnersBucket](https://learnersbucket.com/examples/interview/handle-race-condition-in-react/)

---
