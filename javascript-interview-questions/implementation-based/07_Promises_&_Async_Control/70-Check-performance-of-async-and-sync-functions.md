
---

> [!quote] Metadata  
> **Posted on:** June 2, 2025  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #performance

---

# Check Performance of Async and Sync Functions

Learn how to check the performance of async and sync functions in JavaScript. This question was asked in Atlassian's frontend interview.

---

## Implementation

```javascript
async function measurePerformance(fn, options = {}) {
  const {
    name = fn.name || 'Anonymous Function',
    iterations = 1,
    warmup = true,
    logResults = true,
  } = options;

  const results = {
    name,
    iterations,
    isAsync: fn.constructor.name === 'AsyncFunction',
    timings: [],
    average: 0,
    min: Infinity,
    max: -Infinity,
    total: 0,
  };

  // if warmup enabled
  if (warmup) {
    try {
      await fn();
    } catch (error) {
      console.warn(`Warmup run failed for ${name}:`, error);
    }
  }

  // execute the function for the defined iterations
  for (let i = 0; i < iterations; i++) {
    const start = performance.now();
    try {
      await fn();
    } catch (error) {
      console.error(`Error in iteration ${i + 1} for ${name}:`, error);
      continue;
    }
    const end = performance.now();
    const duration = end - start;

    // compute and store the results
    results.timings.push(duration);
    results.min = Math.min(results.min, duration);
    results.max = Math.max(results.max, duration);
    results.total += duration;
  }

  // calculate averages
  results.average = results.total / results.timings.length;

  // log results
  if (logResults) {
    console.log(`\nPerformance Results for ${name}:`);
    console.log('----------------------------------------');
    console.log(`Type: ${results.isAsync ? 'Async' : 'Sync'}`);
    console.log(`Iterations: ${iterations}`);
    console.log(`Average: ${results.average.toFixed(2)}ms`);
    console.log(`Min: ${results.min.toFixed(2)}ms`);
    console.log(`Max: ${results.max.toFixed(2)}ms`);
    console.log('----------------------------------------\n');
  }

  return results;
}
```

---

## Compare Performance of Two Functions

```javascript
async function comparePerformance(functions, options = {}) {
  const { logResults } = options;
  const results = [];

  for (const { fn, name } of functions) {
    const result = await measurePerformance(fn, { ...options, name });
    results.push(result);
  }

  // sort results by average time
  results.sort((a, b) => a.average - b.average);

  if (logResults !== false) {
    console.log('\nPerformance Comparison:');
    console.log('----------------------------------------');
    results.forEach((result, index) => {
      console.log(`${index + 1}. ${result.name}:`);
      console.log(`   Average: ${result.average.toFixed(2)}ms`);
      console.log(`   Min: ${result.min.toFixed(2)}ms`);
      console.log(`   Max: ${result.max.toFixed(2)}ms`);
    });
    console.log('----------------------------------------\n');
  }

  return results;
}
```

---

## Test Case

```javascript
const syncFunction = () => {
  let sum = 0;
  for (let i = 0; i < 1000000; i++) {
    sum += i;
  }
  return sum;
};

const asyncFunction = async () => {
  await new Promise((resolve) => setTimeout(resolve, 100));
  return 'done';
};

await comparePerformance([
  { fn: syncFunction, name: 'Sync Calculation' },
  { fn: asyncFunction, name: 'Async Operation' },
], {
  iterations: 5,
  warmup: true,
});
```

---

> [!tip] Key Points
> - Use `performance.now()` for accurate timing.
> - Run a **warmup** iteration to ensure no initial errors.
> - Track **min**, **max**, **average**, and **total** timings.
> - Detect async functions with `fn.constructor.name === 'AsyncFunction'`.

---

> [!summary] Takeaway
> - Use `performance.now()` to measure execution time.
> - Run **multiple iterations** for accurate averages.
> - Include a **warmup** run to exclude initial overhead.
> - Compare functions by **average execution time**.

---

### 📎 Reference

[Check performance of async and sync functions - LearnersBucket](https://learnersbucket.com/examples/interview/check-performance-of-async-and-sync-functions-in-javascript/)

---
