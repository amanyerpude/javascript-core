
--------------------------------------------------------------------------
URL : https://learnersbucket.com/examples/interview/implement-clearalltimeout-in-javascript/
# Implement clearAllTimeout in JavaScript

Posted on [February 4, 2022](https://learnersbucket.com/examples/interview/implement-clearalltimeout-in-javascript) | by [Prashant Yadav](https://learnersbucket.com/author/know_prashant/)

Posted in [Interview](https://learnersbucket.com/examples/topics/interview/), [Javascript](https://learnersbucket.com/examples/topics/javascript/) | Tagged [function](https://learnersbucket.com/tag/function/)

It is an extremely common question asked in JavaScript Interviews, where we are asked to implement the clearAllTimeout function. This question was asked in [Meta’s frontend interview](https://leetcode.com/discuss/interview-experience/5027017/Meta-or-IC5-E5-or-Frontend-or-London-or-Rejected).

ClearAllTimeout clears all the setTimeout which are active.

[setTimeout](https://learnersbucket.com/examples/javascript/javascript-settimeout-method) is an asynchronous function that executes a function or a piece of code after a specified amount of time.

`setTimeout` method returns a unique Id when it is invoked, which can be used to cancel the timer anytime using the `clearTimeout` method which is inbuilt.

Reading about the problem statement we can understand that all we have to do is to clear all the active timers and the same can be done by clearing all timeoutIds using `clearTimeout`.

window.clearAllTimeout = function(){
  //clear all timeouts
  while(timeoutIds.length){
    clearTimeout(timeoutIds.pop());
  }
}

But to clear all the timeoutIds at once, we will need to store them somewhere, let’s say in an array. For which we will override the existing `setTimeout` method and collect all the timeoutIds in an array.

window.timeoutIds = [];

// store the original method
const originalTimeoutFn = window.setTimeout;

//over-writing the original method
window.setTimeout = function(fn, delay) { 
  const id = originalTimeoutFn(fn, delay);
  timeoutIds.push(id);
  
  //return the id so that it can be originally cleared
  return id;
}

### Complete code

window.timeoutIds = [];

// store the original method
const originalTimeoutFn = window.setTimeout;

//over-writing the original method
window.setTimeout = function(fn, delay) { 
  const id = originalTimeoutFn(fn, delay);
  timeoutIds.push(id);
  
  //return the id so that it can be originally cleared
  return id;
}

window.clearAllTimeout = function(){
  //clear all timeouts
  while(timeoutIds.length){
    clearTimeout(timeoutIds.pop());
  }
}

If we test this, this runs as expected. It will clear all the timeouts, as `setTimeout` is an Asynchronous function, meaning that the timer function will not pause execution of other functions in the functions stack, thus `clearAllTimeout` runs and cancels them before they can be executed.

setTimeout(() => {console.log("hello")}, 2000);
setTimeout(() => {console.log("hello1")}, 3000);
setTimeout(() => {console.log("hello2")}, 4000);
setTimeout(() => {console.log("hello3")}, 5000);

clearAllTimeout();

If you notice, here we have added a global variable `timeoutIds` which we are using to store the ids of each `setTimeout` and later to cancel all of them, using the global variable is bad practice as it can be overridden.

One thing you could do over here is to wrap these inside a closure or higher-order function or an Object to keep it restricted.

This way we won’t be interfering with existing methods and can still get our work done.

const MY_TIMERS = {
    timeoutIds : [],//global timeout id arrays
    //create a MY_TIMERS's timeout
    setTimeout : function(fn,delay){
        let id = setTimeout(fn,delay);
        this.timeoutIds.push(id);
        return id;
    },
    //MY_TIMERS's clearAllTimeout
    clearAllTimeout : function(){
        while(this.timeoutIds.length){
          clearTimeout(this.timeoutIds.pop());
        }
    }
};

Input:
const id = MY_TIMERS.setTimeout(() => {console.log("hello")}, 1000);
console.log(id);
MY_TIMERS.clearAllTimeout();

Output:
13 //timeoutId

--------------------------------------------------------------------------