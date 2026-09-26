
--------------------------------------------------------------------------
URL : https://learnersbucket.com/examples/interview/throttle-an-array-of-task/

Video_URL : https://youtu.be/4iu9rQBCAGg?si=20GIA9aJnWepAi_V

# Throttle an array of task

Posted on [August 10, 2023](https://learnersbucket.com/examples/interview/throttle-an-array-of-task) | by [Prashant Yadav](https://learnersbucket.com/author/know_prashant/)

Posted in [Interview](https://learnersbucket.com/examples/topics/interview/), [Javascript](https://learnersbucket.com/examples/topics/javascript/)

Implement a throttler that executes an array of tasks. When the throttler is passed a number, only executes that number of the tasks and passes the other tasks into a queue.

#### Example

Input:
const task = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const count = 5;

throttle(task, count, 2000); // [1, 2, 3, 4, 5] // immediately 
throttle(task, count, 2000); // [6, 7, 8, 9, 10] // after 2 seconds
throttle(task, count, 2000); // [1, 2, 3, 4, 5] // after 2 seconds 

In each call, 10 new tasks are pushed and only 5 are executed, remaining are stored in the queue.

## What is a throttling?

Throttling is a way/technique to restrict the number of function execution/calls.

## Throttling an array of tasks.

For this implementation, we will modify the [original throttle function](https://learnersbucket.com/examples/interview/what-is-throttling-in-javascript) to accept an array of tasks and a count and run the number of tasks the same as the count.

In the original implementation, we will add a queue array that will store the task. In each throttle call, copy all the tasks to the queue and then run only the count of tasks from the queue and so on in the consecutive call.

By default, the count will be of the same size as the array of tasks.

const throttle = (task, count = task.length, callback, delay = 1000) => {
  // track the throttle
  let lastFunc;
  let lastRan;
  
  // track the task
  let queue = [];
  
  return function() {
    // store the context to pass it to the callback function
    const context = this;
    const args = arguments; 
    
    // if the throttle is executed the first time
    // run it immediately
    if (!lastRan) {
      // copy all the tasks to the queue
      queue = [...queue, ...task];
      
      // get the amount of task to run
      const execute = queue.splice(0, count);
      
      // pass those tasks to the callback
      callback(execute); 
      
      // update the last ran time
      // to run it after the delay
      lastRan = Date.now();
    } else {
      // clear the timer 
      clearTimeout(lastFunc);
      
      // start a new timer
      // run the function after the delay
      lastFunc = setTimeout(function() {
        // calc the difference between // the last ran and current time
        // if it is greater than the delay // invoke it
        if ((Date.now() - lastRan) >= delay) {
          // copy all the tasks to the queue
          queue = [...queue, ...task];
          
           // get the amount of task to run
          const execute = queue.splice(0, count);
          
          // pass those tasks to the callback
          callback(execute);
          
          // update the last ran time
          // to run it after the delay
          lastRan = Date.now();
        }
      }, delay - (Date.now() - lastRan));
    }
  }
};

Input:
// this will add these tasks at each call
btn.addEventListener('click',  throttle([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 2, (task) => {
  console.log(task);
}, 2000));

Output:
// [object Array] (2)
[1,2] // 1st call

// [object Array] (2)
[3,4] // 2nd call after 2 seconds

// [object Array] (2)
[5,6] // 3rd call after 2 seconds

// [object Array] (2)
[7,8] // 4th call after 2 seconds

// [object Array] (2)
[9,10] // 5th call after 2 seconds

// [object Array] (2)
[1,2] // 6th call after 2 seconds

--------------------------------------------------------------------------