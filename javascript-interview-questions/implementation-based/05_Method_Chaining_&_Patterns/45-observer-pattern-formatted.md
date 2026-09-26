---

> [!quote] Metadata  
> **Posted on:** April 18, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, Javascript  
> **Tags:** #DesignPattern

---

# Observer design pattern in JavaScript

Implement the **Observer design pattern** in JavaScript, also known as pub/sub pattern (publication/subscription). Create a system where objects can subscribe to events and get notified when those events occur.

---

> [!example] Example
> 
> ```javascript
> // Subscribe to events
> move.subscribe(moveHandler);
> move.fire('event #1');
> 
> // Unsubscribe from events
> move.unsubscribe(moveHandler);
> move.fire('event #2');
> 
> // Multiple subscribers
> move.subscribe(moveHandler);
> move.subscribe(moveHandler2);
> move.fire('event #3');
> ```

---

## Concept

Observer design pattern in JavaScript, also known as **pub/sub pattern** short for publication/subscription. It is clear from the name itself that if you are subscribed to the publication and if something is published in the publication it will notify the subscriber.

A **subscription model** in which an object subscribes to a host and the host notifies the object whenever an event occurs is known as the observer pattern. It is one of the important pillars of **event-driven programming** and JavaScript is one of the most popular event-driven programming languages. This pattern promotes **loose coupling** facilitating good object-oriented design.

JavaScript is the most avid user follower observer pattern and we use it almost regularly while building web apps. We write **event handlers** by creating event listeners that listen to an event and notify them every time the event is fired with some additional details of the event.

For example, when a click event is triggered you can access the event object to get all the event details about the click like its position on the screen, etc.

You can also **remove the listener** (unsubscribe) to stop listening if you want.

### Participants

To create the observer design pattern, we need to have **two types of participants**:

1. **Host**
   - It will maintain the list of observers
   - Provides option to subscribe and unsubscribe to the observers
   - Notifies the observer when state changes

2. **Observer**
   - Has a function that gets called/invoked every time a state changes

---

## Implementation

Keeping these two things in mind, we can create the Observer design pattern in JavaScript.

```javascript
const Move = function(){
  this.handlers = [];
  
  this.subscribe = function (fn) {
    this.handlers.push(fn);
  };

  this.unsubscribe = function (fn) {
    this.handlers = this.handlers.filter((item) => item !== fn);
  };

  this.fire = function (o, thisObj) {
    const scope = thisObj || window;
    this.handlers.forEach((item) => {
      item.call(scope, o);
    });
  }
}
```

---

## Input / Output

```javascript
// 1st observer
const moveHandler = function (item) {
  console.log("fired: " + item);
};

// 2nd observer
const moveHandler2 = function (item) {
  console.log("Moved: " + item);
};

const move = new Move();

// subscribe 1st observer
move.subscribe(moveHandler);
move.fire('event #1');

// unsubscribe 1st observer
move.unsubscribe(moveHandler);
move.fire('event #2');

// subscribe 1st & 2nd observer
move.subscribe(moveHandler);
move.subscribe(moveHandler2);
move.fire('event #3');
```

**Output:**
```javascript
"fired: event #1"

"fired: event #3"

"Moved: event #3"
```

---

> [!tip] Key Insight
> 
> - **Observer pattern** = **pub/sub pattern** for loose coupling between objects.
>     
> - **Host** maintains observer list and handles subscribe/unsubscribe operations.
>     
> - **Observers** are functions that get called when events fire.
>     
> - Uses **`forEach`** and **`call`** to invoke all subscribed observers.
>     
> - **`filter`** method removes specific observers during unsubscribe.
>     
> - Essential for **event-driven programming** in JavaScript.

---

> [!summary] Takeaway  
> The Observer pattern enables loose coupling between objects by allowing multiple observers to subscribe to events and get notified when those events occur, making it fundamental to JavaScript's event-driven architecture.

---

### 🔗 Related Resources

[Read Article on LearnersBucket](https://learnersbucket.com/examples/interview/observer-design-pattern-in-javascript/)

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/_gI-rIa6pGI?si=hB4gcobIft24I_h_)

---
