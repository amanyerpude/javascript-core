
---

> [!quote] Metadata  
> **Posted on:** April 1, 2023  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #array #event #function

---

# Array with Event Listeners in JavaScript

Extend arrays in JavaScript such that an event gets dispatched whenever an item is **added** or **removed**.

---

## Problem Statement

- Add the ability to **listen for events** on arrays.
    
- Events supported:
    
    - **add** → triggered when new items are pushed.
        
    - **remove** → triggered when items are popped.
        
- Allow adding/removing listeners dynamically.
    

---

> [!example] Example
> 
> ```javascript
> const arr = [];
> 
> arr.addListener('add', (eventName, items, array) => {
>   console.log('items were added', items);
> });
> 
> arr.addListener('remove', (eventName, item, array) => {
>   console.log(item, ' was removed');
> });
> 
> arr.pushWithEvent('add', [4, 5]);
> arr.popWithEvent('remove');
> ```
> 
> **Output:**
> 
> ```javascript
> "items were added" [4, 5]
> 5  " was removed"
> ```

---

## Implementation

```javascript
// track the events and their callbacks
Array.prototype.listeners = {};

// add/assign a new event with listener
Array.prototype.addListener = function(name, callback) {
  if (!this.listeners[name]) {
    this.listeners[name] = [];
  }
  this.listeners[name].push(callback);
};

// trigger event on push
Array.prototype.pushWithEvent = function(event, args) {
  this.push(...args);
  this.triggerEvent(event, args);
};

// trigger event on pop
Array.prototype.popWithEvent = function(event) {
  const element = this.pop();
  this.triggerEvent(event, element);
};

Array.prototype.triggerEvent = function(eventName, elements) {
  if (this.listeners[eventName]) {
    this.listeners[eventName].forEach(callback =>
      callback(eventName, elements, this)
    );
  }
};

// remove listener
Array.prototype.removeListener = function(eventName, callback) {
  if (this.listeners[eventName]) {
    this.listeners[eventName] =
      this.listeners[eventName].filter((e) => e !== callback);
  }
};
```

---

## Input / Output

```javascript
const arr = [];

const onAdd = (eventName, items, array) => {
  console.log('items were added', items);
};

const onAddAgain = (eventName, items, array) => {
  console.log('items were added again', items);
};

arr.addListener('add', onAdd);
arr.addListener('add', onAddAgain);

arr.addListener('remove', (eventName, item, array) => {
  console.log(item, ' was removed');
});

arr.pushWithEvent('add', [1, 2, 3, 'a', 'b']);

arr.removeListener('add', onAddAgain); // removes the second listener

arr.pushWithEvent('add', [4, 5]);
arr.popWithEvent('remove');

console.log(arr);
```

**Output:**

```javascript
"items were added" [1, 2, 3, "a", "b"]
"items were added again" [1, 2, 3, "a", "b"]

"items were added" [4, 5]

5  " was removed"

[1, 2, 3, "a", "b", 4]
```

---

> [!tip] Key Insights
> 
> - `removeListener` does **not** work for anonymous functions (because they cannot be referenced for removal).
>     
> - Useful for learning how **events, callbacks, and prototype extension** work in JavaScript.
>     
> - Don’t use in production — extending native prototypes can cause conflicts.
>     

---

> [!summary] Takeaway  
> By extending the `Array.prototype`, we can simulate event-driven behavior on arrays — learning about **custom events**, **prototypes**, and **observer patterns** in JavaScript.

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/3jKvw-iZOXk)

---
