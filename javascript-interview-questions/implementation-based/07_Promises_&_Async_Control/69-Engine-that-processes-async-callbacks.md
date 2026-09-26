
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** FrontendLead  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #meta

---

# Implement an Engine that Processes Async Callbacks

Implement a `QueueCallbacks` class that manages async callbacks with concurrency control. Your task is to provide the implementation of the constructor and process methods.

---

## Example

```javascript
const queue = new QueueCallbacks(2);

queue.process(() => fetchUser(1));   // starts immediately
queue.process(() => fetchUser(2));   // starts immediately
queue.process(() => fetchUser(3));   // waits in queue
queue.process(() => fetchUser(4));   // waits in queue
```

---

## Implementation

```javascript
class QueueCallbacks {
  constructor(concurrency) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }

  process(fn) {
    if (this.running < this.concurrency) {
      this.execute(fn);
    } else {
      this.queue.push(fn);
    }
  }

  execute(fn) {
    this.running++;
    Promise.resolve()
      .then(() => fn())
      .finally(() => {
        this.running--;
        if (this.queue.length > 0) {
          const next = this.queue.shift();
          this.execute(next);
        }
      });
  }
}
```

---

## How It Works

1. **Constructor** takes a concurrency limit.
2. **process()** checks if below limit, executes or queues.
3. **execute()** increments counter, runs the function, then decrements.
4. On completion, **dequeue** next task if available.

---

> [!tip] Key Points
> - This is a **task queue** with concurrency control.
> - Uses `Promise.resolve().then()` to ensure async execution.
> - `.finally()` handles both success and failure cases.
> - Tasks are processed in **FIFO** order.

---

> [!summary] Takeaway
> - Implement a **concurrency-limited** async task processor.
> - Use a **queue** for overflow tasks.
> - Process next task on **completion** of current one.
> - Ensure async execution with `Promise.resolve().then()`.

---

### 📎 Reference

[Meta - Frontend - Engine that process async callbacks - FrontendLead](https://discuss.frontendlead.com/t/meta-frontend-question-engine-that-process-async-callbacks/1430)

---
