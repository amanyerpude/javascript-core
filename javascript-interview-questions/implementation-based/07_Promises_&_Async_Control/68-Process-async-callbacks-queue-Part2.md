
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** LearnersBucket  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #meta

---

# Process Async Callbacks Queue – Part 2

Implement a class that processes async callbacks with a concurrency limit. If there are two async functions currently being executed, the next callback method should be put into the queue. After one of the currently executing async functions is finished, the next one from the queue should start.

---

## Example

```javascript
const queue = new QueueCallbacks(2);

queue.callback(() => asyncTask1());
queue.callback(() => asyncTask2());
queue.callback(() => asyncTask3());
queue.callback(() => asyncTask4());

// asyncTask1 and asyncTask2 start immediately
// asyncTask3 and asyncTask4 wait in queue
// When asyncTask1 finishes, asyncTask3 starts
// When asyncTask2 finishes, asyncTask4 starts
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

  callback(fn) {
    if (this.running < this.concurrency) {
      this.run(fn);
    } else {
      this.queue.push(fn);
    }
  }

  run(fn) {
    this.running++;
    fn().then(() => {
      this.running--;
      if (this.queue.length > 0) {
        const next = this.queue.shift();
        this.run(next);
      }
    });
  }
}
```

---

## How It Works

1. **Track** the number of currently running async tasks.
2. If below **concurrency limit**, execute immediately.
3. If at limit, **queue** the callback.
4. When a task **finishes**, decrement counter and start next from queue.

---

> [!tip] Key Points
> - This is a **concurrency limiter** pattern.
> - Similar to how thread pools work in other languages.
> - Uses a simple **queue** (FIFO) for waiting tasks.
> - The `run` method is **recursive** via queue processing.

---

> [!summary] Takeaway
> - Process async callbacks with a **concurrency limit**.
> - Use a **queue** to hold waiting tasks.
> - Process next task when current one **completes**.
> - Useful for **rate-limiting** API calls or file operations.

---

### 📎 Reference

[Process Async Callbacks Queue - YouTube (LearnersBucket)](https://m.youtube.com/watch?v=5FgOiXAUpkE)

---
