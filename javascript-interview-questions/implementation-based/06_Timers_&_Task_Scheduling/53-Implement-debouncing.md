
--------------------------------------------------------------------------
URL : https://learnersbucket.com/examples/interview/what-is-debouncing-in-javascript/

# What is debouncing in javascript?

Posted on [January 1, 2020](https://learnersbucket.com/examples/interview/what-is-debouncing-in-javascript) | by [Prashant Yadav](https://learnersbucket.com/author/know_prashant/)

Posted in [Interview](https://learnersbucket.com/examples/topics/interview/), [Javascript](https://learnersbucket.com/examples/topics/javascript/) | Tagged [Easy](https://learnersbucket.com/tag/easy/), [SystemDesign](https://learnersbucket.com/tag/systemdesign/)

Learn what is debouncing in javascript.

Optimizing performance often crops up in javascript application. Well, there many things that need to be considered while optimizing an app.

Excessively invoking the function majorly hampers the performance in javascript and considered as one of the key hurdles.

There are scenarios where we may invoke functions when it isn’t necessary. For example, consider a callback function that we want to execute on the window resize. It does not make any sense to call the function as we keep resizing.

We need to execute the [callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) function only when the resizing is finished.

Debouncing and [Throttling](https://learnersbucket.com/examples/javascript/what-is-throttling-in-javascript) help us to gain control over the rate at which function is called or executes.

## What exactly is debouncing in javascript?

Debouncing is a method or a way to execute a function when it is made sure that no further repeated event will be triggered in a given frame of time.

A technical example is when we search something on any eCommerce site, we don’t want to trigger search function and make a request to the server as the user keeps typing each letter. We want the user to finish typing and then wait for a specified window of time to see if the user is not going to type anything else or has finished typing and return the result.

Here is the code to implement the debounce function in javascript.

const debounce = (func, delay) => {
  let inDebounce;
  return function() {
    const context = this;
    const args = arguments;
    clearTimeout(inDebounce);
    inDebounce = setTimeout(() => func.apply(context, args), delay);
  };
};

We the code is self-explanatory but let me explain it as well.

We created a function that will return a function. The outer function uses a variable to keep track of time for the execution of the inner function.

The inner function will be called only after a specified window of time, to achieve this we use [setTimeout](https://learnersbucket.com/examples/javascript/javascript-settimeout-method) function `inDebounce = setTimeout(() => func.apply(context, args), delay);`.

We store the reference of delayed function so that we can clear it if the outer function is re-invoked before the specified time `clearTimeout(inDebounce)` and again recall it.

If we are invoking for the first time, our function will execute at the end of our delay. If we invoke and then reinvoke again before the end of our delay, the delay restarts.

Here is how we can use debounce in javascript.

btn.addEventListener('click', debounce(function() {
  console.info('HOLA! oppo', new Date().toUTCString());
}, 3000));

--------------------------------------------------------------------------