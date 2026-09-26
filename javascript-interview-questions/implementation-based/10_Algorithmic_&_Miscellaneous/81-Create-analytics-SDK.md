
---

> [!quote] Metadata  
> **Posted on:** July 6, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #flipkart

---

# Create Analytics SDK in JavaScript

Implement an analytics SDK that exposes log events, it takes in events and queues them, and then starts sending the events. This is a Flipkart frontend interview question.

- Send each event after a delay of 1 second and this logging fails every n % 5 times.
- Send the next event only after the previous one resolves.
- When the failure occurs attempt a retry.

---

## Example

```javascript
const sdk = new SDK();
sdk.logEvent("event 1");
sdk.logEvent("event 2");
sdk.logEvent("event 3");
sdk.logEvent("event 4");
sdk.logEvent("event 5");
sdk.send();

// Output:
// "Analytics sent event 1"
// "Analytics sent event 2"
// "Analytics sent event 3"
// "Analytics sent event 4"
// -----------------------
// "Failed to send event 5"
// "Retrying sending event 5"
// -----------------------
// "Analytics sent event 5"
```

---

## Implementation

```javascript
class SDK {
  constructor() {
    // hold the events
    this.queue = [];
    // track the count
    this.count = 1;
  }

  // push event in the queue
  logEvent(ev) {
    this.queue.push(ev);
  }

  // function to delay the execution
  wait = () =>
    new Promise((resolve, reject) => {
      setTimeout(() => {
        // reject every n % 5 time
        if (this.count % 5 === 0) {
          reject();
        } else {
          resolve();
        }
      }, 1000);
    });

  // recursively send the events
  sendAnalytics = async function () {
    // base case: no more events
    if (this.queue.length === 0) {
      return;
    }

    // get the first element from the queue
    const current = this.queue.shift();

    try {
      await this.wait();
      console.log("Analytics sent " + current);
      this.count++;
    } catch (e) {
      console.log("-----------------------");
      console.log("Failed to send " + current);
      console.log("Retrying sending " + current);
      console.log("-----------------------");
      // reset the count
      this.count = 1;
      // push the event back for retry
      this.queue.unshift(current);
    } finally {
      // recursively send remaining
      this.sendAnalytics();
    }
  };

  // start the execution
  send = async function () {
    this.sendAnalytics();
  };
}
```

---

## How It Works

1. **Queue** stores events to be sent.
2. **Count** tracks the number of events sent (for n%5 failure simulation).
3. **wait()** delays execution by 1s and rejects every 5th call.
4. **sendAnalytics()** processes events one by one recursively.
5. On failure, the event is **pushed back** to the front of the queue for retry.
6. `finally` block ensures **recursive** processing continues.

---

> [!tip] Key Points
> - Events are sent **sequentially** with a 1-second delay.
> - Failure occurs every **n%5** times (simulated).
> - Failed events are **retried** by pushing back to queue.
> - Uses **recursion** to process the queue.
> - `count` resets to 1 after a failure.

---

> [!summary] Takeaway
> - Queue events and process them **sequentially**.
> - Add **delay** between events using `setTimeout`.
> - Implement **retry logic** by re-queuing failed events.
> - Use **recursion** for continuous queue processing.
> - Track event count for **failure simulation**.

---

### 📎 Reference

[Create analytics SDK - LearnersBucket](https://learnersbucket.com/examples/interview/create-analytics-sdk-in-javascript/)

---
