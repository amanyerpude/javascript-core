---

> [!quote] Metadata  
> **Posted on:** April 26, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, Javascript  
> **Tags:** #async

---

# Execute async functions in Series

Implement a function that takes a list of **async functions** as input and executes them in a series that is one at a time. The next task is executed only when the previous task is completed.

---

> [!example] Example
> 
> ```javascript
> [
>   asyncTask(3),
>   asyncTask(1),
>   asyncTask(2)
> ]
> 
> // Output:
> // 3
> // 1
> // 2
> ```

---

## Concept

One of the complex parts of working with JavaScript is handling the **async code** and the same is tested in interviews to make sure you are an experienced developer who can handle it.

We will see **four different approaches** to solve this problem:

1. **Using async/await** - Modern ES6 approach with try-catch
2. **With recursion** - Recursive function calls
3. **Using Array.reduce** - Functional programming approach
4. **Unbounded async tasks** - Handling non-promise async tasks

---

## Implementation

### Using async/await

async/await is the new keyword introduced in ES6 that helps us to handle the promises by avoiding the callback chaining.

For…of loop allows using await keyword performing the next iteration only when the previous one is finished.

To handle the rejection, we wrap the async/await in the try-catch block.

```javascript
const asyncSeriesExecuter = async function(promises) {
  for (let promise of promises) {
    try{
      const result = await promise();
      console.log(result);
    }catch(e){
      console.log(e);
    }
  }
}
```

### With Recursion

We can execute the async tasks in series by recursively calling the same function after the current task is executed.

```javascript
const asyncSeriesExecuter = function(promises) {
  // get the top task
  let promise = promises.shift();
  
  //execute the task
  promise().then((data) => {
    //print the result
    console.log(data);
    
    //recursively call the same function
    if (promises.length > 0) {
      asyncSeriesExecuter(promises);
    }
  });
}
```

### Using Array.reduce

Rather than calling the function recursively, we can use the `Array.reduce` to do the execution in series.

```javascript
const asyncSeriesExecuter = function(promises) {
  promises.reduce((acc, curr) => {
    return acc.then(() => {
      return curr().then(val => {console.log(val)});
    });
  }, Promise.resolve());
}
```

### Unbounded Async Tasks

In case the async task is not wrapped in promises, you can wrap them explicitly to run them in series.

**Unwrapped async tasks:**

```javascript
//simply returns as setTimeout
const asyncTask = function(i) {
  return function(callback){
    setTimeout(() => {
      callback(`Completing ${i}`)
    }, 100*i);
  };
}

const tasks = [
  asyncTask(3),
  asyncTask(1),
  asyncTask(7),
  asyncTask(2),
  asyncTask(5),
];
```

**To run them in series we can create a new promise every time and resolve it at the end of execution of setTimeout:**

```javascript
const asyncSeriesExecuter = function(tasks) {
  tasks.reduce((acc, curr) => {
    //return the last executed task
    return acc.then(() => {
      //create a new promise
      return new Promise((resolve, reject) => { 
        //exec the async task
        curr(val => {
          console.log(val);
          //resolve after task is completed
          resolve();
        }); 
      });
    });
  }, Promise.resolve());
}
```

---

## Input / Output

### Method 1: async/await

```javascript
const asyncTask = function(i) {
 return function(){
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(`Completing ${i}`), 100*i)
  });
 }
}

const promises = [
  asyncTask(3),
  asyncTask(1),
  asyncTask(7),
  asyncTask(2),
  asyncTask(5),
];

asyncSeriesExecuter(promises);
```

**Output:**
```javascript
"Completing 3"
"Completing 1"
"Completing 7"
"Completing 2"
"Completing 5"
```

### Method 2: Recursion

```javascript
const asyncTask = function(i) {
  return function(){
   return new Promise((resolve, reject) => {
    setTimeout(() => resolve(`Completing ${i}`), 100*i)
   });
 }
}

const promises = [
  asyncTask(3),
  asyncTask(1),
  asyncTask(7),
  asyncTask(2),
  asyncTask(5),
];

asyncSeriesExecuter(promises);
```

**Output:**
```javascript
"Completing 3"
"Completing 1"
"Completing 7"
"Completing 2"
"Completing 5"
```

### Method 3: Array.reduce

```javascript
const asyncTask = function(i) {
  return function(){
   return new Promise((resolve, reject) => {
    setTimeout(() => resolve(`Completing ${i}`), 100*i)
   });
 }
}

const promises = [
  asyncTask(3),
  asyncTask(1),
  asyncTask(7),
  asyncTask(2),
  asyncTask(5),
];

asyncSeriesExecuter(promises);
```

**Output:**
```javascript
"Completing 3"
"Completing 1"
"Completing 7"
"Completing 2"
"Completing 5"
```

### Method 4: Unbounded Async Tasks

```javascript
const asyncTask = function(i) {
  return function(callback){
    setTimeout(() => {
      callback(`Completing ${i}`)
    }, 100*i);
  };
}

const tasks = [
  asyncTask(3),
  asyncTask(1),
  asyncTask(7),
  asyncTask(2),
  asyncTask(5),
];

asyncSeriesExecuter(tasks);
```

**Output:**
```javascript
"Completing 3"
"Completing 1"
"Completing 7"
"Completing 2"
"Completing 5"
```

---

> [!tip] Key Insight
> 
> - **async/await**: Most readable, uses try-catch for error handling.
>     
> - **Recursion**: Uses `promises.shift()` to process tasks one by one.
>     
> - **Array.reduce**: Functional approach, chains promises sequentially.
>     
> - **Unbounded tasks**: Wraps non-promise async functions in promises.
>     
> - All methods ensure **sequential execution** - next task waits for previous completion.

---

> [!summary] Takeaway  
> Sequential async execution can be achieved through multiple approaches, with async/await being the most readable, while Array.reduce offers a functional programming solution, and custom promise wrapping handles non-promise async tasks.

---

### 🔗 Related Article

[Read on LearnersBucket](https://learnersbucket.com/examples/interview/execute-async-functions-in-series/)

---
