

--------------------------------------------------------------------------
Video_URL : https://youtu.be/xNf_jEKz13Q?si=4QuTt6gJCiyRQ9G8

This question was asked in Hive's 2025 Frontend Interview Test - [https://learnersbucket.com/wp-content...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXpqak5tU0NoMUZJMGxOSFlmdDVGRG44R01kUXxBQ3Jtc0ttdHlTYmpkN0ZyNmRoLWJjZlVGRVotQTJweXVXWTB3a0x0R3FPQjFUMThiQXN5cEVCdURwQnlQSzUxTlFYQWJNSjhldHBCRkhzSEcxX2hRLTc2VWFIV0NKbkFoYmdJM0VteTQ1QkFUQU4xaFp5WW51cw&q=https%3A%2F%2Flearnersbucket.com%2Fwp-content%2Fuploads%2F2025%2F08%2FHive-Frontend-Developer-Test-2025.pdf&v=xNf_jEKz13Q). The problem statement reads as:- To create a JavaScript class that accepts an array of functions. The functions can be async functions. The class exposes methods to execute the array of promises in serial (one by one) as well as pause the execution. Methods: run() – Executes the array of functions in serial. async pause() – Pauses the execution of the functions. getState() – Returns the current state – InProgress | Paused | Completed and unexecuted functions indices. runAllUnexecutedFunctions() – Executes all unexecuted functions.

--------------------------------------------------------------------------

--------------------------------------------------------------------------

This is a comprehensive reconstruction of the **Promise Scheduler** coding challenge. I have scraped the text from all images and organized it into a clean, structured format optimized for Obsidian.

---

# Promise Scheduler Challenge

## 1. Problem Statement

The goal is to create a **JavaScript Class** that accepts an array of functions (which may be asynchronous). The class exposes methods to execute these functions in **serial** (one by one) while providing the ability to **pause** execution.

### Key Requirements

- Functions must be executed one by one (sequentially).
    
- The scheduler must support pausing.
    
- After a pause, resuming via `run()` should start exactly where the scheduler left off.
    
- The class must handle both synchronous and asynchronous functions.
    

---

## 2. Constructor & Options

The class should be named `PromiseScheduler`. It accepts an array of functions and an optional `options` object.

### Constructor Signature

JavaScript

```
const promiseSchedulerInstance = new PromiseScheduler(asyncFunctionsWhichReturnPromises, options);
```

### The `options` Object

|**Property**|**Type**|**Description**|
|---|---|---|
|`startIndex`|`number`|The index to start execution from when `run()` is called for the first time. (Default: `0`)|
|`callbacks`|`object`|An object containing lifecycle hooks.|
|`callbacks.onCompleted`|`function`|Triggered when all functions have been resolved or rejected. Receives `arrayOfResultsOfAllFunctions` as an argument.|

---

## 3. Class Methods

You are required to implement the following four methods:

### `run()`

- Starts executing functions in serial starting from the `startIndex` (or the current internal index if resumed).
    
- Does not return anything.
    
- If already `in-progress`, calling this should have no effect.
    

### `async pause()`

- Pauses the scheduler.
    
- **Crucial:** It must `await` the currently executing function to resolve before officially pausing.
    
- Next execution of `run()` begins from the next index in the array.
    

### `getState()`

Returns an object representing the current internal state of the scheduler:

JavaScript

```
{
  status: 'in-progress' | 'paused' | 'completed',
  unexecutedFunctionsIndices: [] // Array of indices not yet executed
}
```

### `runAllUnexecutedFunctions()`

- Triggers the scheduler to execute all pending functions in serial.
    
- Must handle cases where `startIndex` was non-zero.
    
- Has no effect if the scheduler is already in an `in-progress` state.
    

---

## 4. Usage Examples

### Input: Array of Functions

The input array can contain various types of functions that return promises or values.

JavaScript

```
const asyncFunctionsWhichReturnPromises = [
  // Example 1: Standard Promise with setTimeout
  () => new Promise((resolve) => setTimeout(() => resolve(true), 5000)),
  
  // Example 2: Async function using await
  async () => {
    await delay(5000);
    return true;
  },
  
  // Example 3: Image loading Promise
  () => {
    return new Promise((resolve, reject) => {
      const image = new Image();
      image.onload = () => resolve(image);
      image.onerror = () => reject();
      image.src = "somePng.png";
    });
  },
  // ... more functions
];
```

### Execution Flow Example

JavaScript

```
// Initialization
const options = {
  startIndex: 0,
  callbacks: {
    onCompleted: (results) => console.log(results)
  }
};

const scheduler = new PromiseScheduler(asyncFunctionsWhichReturnPromises, options);

// Start execution
scheduler.run();

// Example: Pause after 6 seconds
await delay(6000);
await scheduler.pause();

// Check State
const state = scheduler.getState();
/* Example Output if 3 functions finished before pause:
{
  status: 'paused',
  unexecutedFunctionsIndices: [3, 4, 5]
}
*/

// Resume
scheduler.runAllUnexecutedFunctions();
```

--------------------------------------------------------------------------



---

# 🧪 Senior Frontend Machine Coding Interview Question

---

## 🧩 Part 1 — Core Problem Statement (Initial Prompt)

> We want you to design a JavaScript class called `PromiseScheduler`.

It should:

1. Accept an array of functions.
    
2. Each function may be:
    
    - Synchronous
        
    - Asynchronous
        
    - Returning a Promise
        
3. When executed, they must run **strictly in serial** (one after another).
    
4. The class should allow:
    
    - Starting execution
        
    - Pausing execution
        
    - Resuming execution
        
    - Querying current state
        

You are not allowed to use:

```js
Promise.all
```

Execution must be strictly sequential.

---

## 🧠 Part 2 — Constructor Requirements

Your class must accept:

```js
new PromiseScheduler(tasks, options)
```

### Where:

### `tasks`

An array like:

```js
[
  () => Promise.resolve(1),
  async () => 2,
  () => new Promise(res => setTimeout(() => res(3), 1000)),
]
```

Each element is a **function**, not a promise.

---

### `options` (Optional)

It may contain:

```js
{
  startIndex: number,
  callbacks: {
    onCompleted: (results) => void
  }
}
```

#### Behavior:

- `startIndex` determines where execution begins the first time `run()` is called.
    
- `onCompleted` should fire only when ALL tasks have been executed (resolved or rejected).
    

Default:

```
startIndex = 0
```

---

# ⚙️ Required Methods

You must implement the following methods.

---

## 1️⃣ run()

- Starts execution from:
    
    - `startIndex` (first run)
        
    - or last paused index (if resumed)
        
- If already running → do nothing
    
- Must not run tasks in parallel
    
- Must handle rejected promises without crashing
    

---

## 2️⃣ async pause()

This is the most important requirement.

- If called while running:
    
    - It must NOT kill the current promise.
        
    - It must wait for the currently executing function to finish.
        
    - Then stop before starting the next one.
        
- If not running → do nothing
    

This method must be `async`.

---

## 3️⃣ getState()

Must return:

```js
{
  status: 'in-progress' | 'paused' | 'completed',
  unexecutedFunctionsIndices: number[]
}
```

Where:

- `status` reflects scheduler state
    
- `unexecutedFunctionsIndices` are tasks not yet started
    

---

## 4️⃣ runAllUnexecutedFunctions()

- Should execute all remaining tasks
    
- Should behave like resuming execution
    
- If already running → no effect
    

---

# 🧪 Part 3 — Execution Scenario

You will be tested with something like:

```js
scheduler.run();

setTimeout(() => {
  scheduler.pause();
}, 6000);
```

After pausing, we may call:

```js
scheduler.run();
```

Or:

```js
scheduler.runAllUnexecutedFunctions();
```

Execution must continue correctly.

---

# 🧠 Part 4 — Edge Case Clarifications (Interviewer May Ask)

Be ready to answer:

1. What if a task throws synchronously?
    
2. What if pause() is called multiple times?
    
3. What if run() is called twice quickly?
    
4. What if startIndex is 3 in an array of 5?
    
5. What if a task returns a non-promise value?
    
6. What if a task rejects?
    
7. Can we cancel a running task?
    
8. What happens if tasks array is empty?
    

---

# 🔥 Part 5 — Follow-Up (Senior Level)

After you finish basic implementation, interviewer may ask:

### Extension 1:

> Can you modify this to support concurrency = 2?

Now tasks can run 2 at a time but still preserve order of results.

---

### Extension 2:

> How would you make this cancellable?

---

### Extension 3:

> How would you design this as a reusable NPM package?

---

### Extension 4:

> Can you convert this into a hook in React?

---

# 🧠 What They Are ACTUALLY Testing

This question tests:

|Skill|What They Observe|
|---|---|
|Async mastery|Do you understand await in loops?|
|State modeling|Do you use proper status management?|
|Race condition handling|Can you prevent multiple loops?|
|Graceful pausing|Do you store currentPromise?|
|Error handling|Does one failure crash system?|
|Production thinking|Do you consider edge cases?|
|Communication|Do you think out loud?|

---

# 🎤 How a Real Interview Might Start

Interviewer:

> “Let’s build a Promise Scheduler. You have 40 minutes. You may ask clarifying questions before coding.”

You should respond with:

- Clarifying constraints
    
- Explaining approach
    
- Modeling state
    
- Then coding incrementally
    

---

Now we begin practice.

I will switch into interviewer mode.

---

🎤 **Interviewer:**

Before you start coding, what clarifying questions would you ask me about this problem?

Your answer should be structured and confident.



---

# 🧠 STAGE 0 — Calm Opening (0–2 minutes)

### 🎤 What You Say

> "Before jumping into implementation, I want to clarify a few constraints to ensure I’m modeling this correctly."

Then ask:

1. Are tasks guaranteed to be functions?
    
2. Can they return non-promise values?
    
3. Should rejected promises stop execution or continue?
    
4. Should run() return anything?
    
5. Is startIndex circular or linear only forward?
    

Then summarize:

> "So we need a serial task runner with pause/resume capability. Since JavaScript doesn’t support canceling promises natively, pause must be graceful — meaning we wait for the current task to finish before stopping."

---

### 🧠 What Interviewer Is Checking

- Do you clarify constraints?
    
- Do you understand promises can't be cancelled?
    
- Are you thinking architecturally?
    

---

# 🏗 STAGE 1 — Modeling the State (5 minutes)

### 🎤 What You Say

> "Since this is essentially a task scheduler, I’ll first model its internal state before writing execution logic."

Then explain:

We need to track:

- tasks
    
- results
    
- currentIndex
    
- status
    
- currentPromise
    
- callbacks
    

---

### ✍️ What You Write

```js
class PromiseScheduler {
  constructor(tasks, options = {}) {
    this.tasks = tasks;
    this.results = [];

    this.currentIndex = options.startIndex || 0;
    this.onCompleted = options.callbacks?.onCompleted;

    this.status = 'idle'; // idle | in-progress | paused | completed
    this.currentPromise = null;
  }
}
```

---

### 🎤 Then Say

> "I’m using a single `status` variable instead of multiple booleans because this behaves like a finite state machine."

That sentence alone scores senior points.

---

# ⚙️ STAGE 2 — Implement getState() (Warm-up Method)

Start with easiest method first.

### 🎤 What You Say

> "I'll first implement getState() since it helps validate our internal state while developing."

---

### ✍️ What You Write

```js
getState() {
  return {
    status: this.status,
    unexecutedFunctionsIndices: this.tasks
      .map((_, i) => i)
      .filter(i => i >= this.currentIndex)
  };
}
```

---

### 🎤 Then Say

> "I’m returning a new object and a filtered array to prevent external mutation of internal state."

That shows immutability thinking.

---

# 🚀 STAGE 3 — Implement run() (Core Engine)

Now move to main logic.

### 🎤 What You Say BEFORE Coding

> "For run(), I need to ensure:
> 
> - Only one execution loop runs at a time
>     
> - Tasks execute strictly sequentially
>     
> - Rejections don’t crash the scheduler
>     
> - We can pause safely"
>     

---

### ✍️ What You Write

```js
async run() {
  if (this.status === 'in-progress' || this.status === 'completed') {
    return;
  }

  this.status = 'in-progress';

  while (
    this.currentIndex < this.tasks.length &&
    this.status === 'in-progress'
  ) {
    const task = this.tasks[this.currentIndex];

    try {
      this.currentPromise = Promise.resolve(task());
      const result = await this.currentPromise;
      this.results[this.currentIndex] = result;
    } catch (error) {
      this.results[this.currentIndex] = error;
    } finally {
      this.currentIndex++;
    }
  }

  if (this.currentIndex === this.tasks.length) {
    this.status = 'completed';
    if (this.onCompleted) {
      this.onCompleted(this.results);
    }
  }
}
```

---

### 🎤 Pause and Explain

Explain these 3 critical decisions:

---

### ✅ Why `Promise.resolve(task())`?

> "This ensures that even if task returns a non-promise value, we normalize it into a promise."

That’s advanced async understanding.

---

### ✅ Why store `this.currentPromise`?

> "Because pause() needs a reference to await the currently running task."

If you don’t say this, you lose points.

---

### ✅ Why use `while` instead of `forEach`?

> "Because forEach doesn’t respect await correctly, and while gives better control over pause conditions."

---

# ⏸ STAGE 4 — Implement pause() (Critical Section)

This is where most candidates fail.

---

### 🎤 What You Say Before Coding

> "Since JavaScript promises cannot be forcefully cancelled, pause must be cooperative. So I'll change status to 'paused' and await the currently executing promise."

---

### ✍️ What You Write

```js
async pause() {
  if (this.status !== 'in-progress') {
    return;
  }

  this.status = 'paused';

  if (this.currentPromise) {
    await this.currentPromise;
  }
}
```

---

### 🎤 Then Explain

> "Setting status to paused ensures the loop doesn’t start the next task. Awaiting currentPromise ensures graceful stopping."

This shows deep async reasoning.

---

# 🔁 STAGE 5 — Implement runAllUnexecutedFunctions()

Simple alias.

---

### ✍️ What You Write

```js
runAllUnexecutedFunctions() {
  this.run();
}
```

---

### 🎤 What You Say

> "Since currentIndex is preserved, run() automatically resumes from where we paused."

---

# 🧠 STAGE 6 — Edge Case Discussion (Senior Bonus)

After finishing coding, don’t stop.

Start saying:

---

> "Before wrapping up, I want to highlight a few edge cases."

Then mention:

1. Synchronous throws — handled by try/catch
    
2. Multiple run() calls — guarded by status check
    
3. Multiple pause() calls — guarded
    
4. Empty task array — immediately completed
    
5. Rejected promises — captured in results
    
6. startIndex beyond bounds — should validate
    

This is how you elevate from good to senior.

---

# 🏆 What Makes This a 9/10 Answer

- Clarified constraints
    
- Modeled state first
    
- Used finite state machine pattern
    
- Normalized promises
    
- Prevented concurrency bugs
    
- Handled errors
    
- Explained decisions
    

---

# 🔥 If Interviewer Pushes Further

Be ready to answer:

> "How would you support concurrency = 3?"

Answer:

- Maintain activeCount
    
- Use queue
    
- Use Promise.race
    
- Track running promises
    

---

# 🎯 Your Next Training Step

Now I want you to do something important.

I will switch into interviewer mode and ask:

> “Walk me through your run() method. Why is it safe from race conditions?”

You answer.

Let’s start thinking like a senior now.