
---

> [!quote] Metadata  
> **Posted on:** April 5, 2022  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #function

---

# Sampling function in JavaScript

Create a function in JavaScript that accepts a function as input and a count, and executes that input function **once for every given count of calls**.  
This is known as a **sampling function**.

---

> [!example] Example
> 
> ```javascript
> function message() {
>   console.log("hello");
> }
> 
> const sample = sampler(message, 4);
> sample();
> sample();
> sample();
> sample(); // this will be executed
> sample();
> sample();
> sample();
> sample(); // this will be executed
> ```

---

> [!tip] Sampling vs Throttling
> 
> - **Throttling** → limits the execution of a function once in a given amount of time.
>     
> - **Sampling** → limits execution by only running the function once in a given number of calls.
>     

---

## Implementation

To create a sampling function, we can use a **closure** that tracks how many times the function has been called.  
When the counter reaches the specified count, the input function executes and the counter resets.

```javascript
function sampler(fn, count, context) {
    let counter = 0;

    return function(...args) {
        // set the counters
        let lastArgs = args;
        context = this ?? context;
        
        // invoke only when number of calls is equal to the count
        if (++counter !== count) return;
        
        fn.apply(context, args);
        counter = 0;
    };
}
```

---

## Example Run

```javascript
function message() {
  console.log("hello");
}

const sample = sampler(message, 4);
sample();
sample();
sample();
sample(); // hello
sample();
sample();
sample();
sample(); // hello
```

---

> [!summary] Takeaway  
> A **sampling function** executes a given function **once every N calls**, making it useful when you want to control frequency by call count instead of time (as with throttling).

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/e8ZgzldSwYo)

---
