
---

> [!quote] Metadata  
> **Posted on:** October 11, 2025  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #async #intuit

---

# Sum Up Functions Return Value Running in Parallel and in Sequence

This interview question was asked in Intuit's SDE2 frontend interview.

Write two functions:
- `A()` returns 2 after 2 seconds
- `B()` returns 3 after 3 seconds

Return their sum in two ways:
- **Parallel execution** → Total time: 3 seconds
- **Sequential execution** → Total time: 5 seconds

---

## Implementation

```javascript
const wait = (num) => {
  return new Promise((resolve) => setTimeout(resolve, num * 1000, num));
};

const A = async () => {
  return wait(2);
};

const B = async () => {
  return wait(3);
};
```

---

## Sequential Execution

```javascript
const series = async () => {
  const result1 = await A();
  const result2 = await B();
  return result1 + result2;
};
```

## Parallel Execution

```javascript
const parallel = async () => {
  const task1 = A();
  const task2 = B();
  const result1 = await task1;
  const result2 = await task2;
  return result1 + result2;
};
```

---

## Performance Evaluation

```javascript
const evaluate = async (fn, label) => {
  const startTime = performance.now();
  console.log(`Executing ${label} task starts...`);
  let result = await fn();
  const endTime = performance.now();
  console.log(
    `Task ${label} finished in ${Number.parseInt(endTime - startTime)} milliseconds with sum:`,
    result
  );
};

evaluate(series, 'sequential');
evaluate(parallel, 'parallel');

// Output:
// "Task sequential starting..."
// "Task parallel starting..."
// "Task parallel finished in 3029 milliseconds with sum:" 5
// "Task sequential finished in 5011 milliseconds with sum:" 5
```

---

> [!tip] Key Points
> - In **sequential**, each function starts only after the previous finishes.
> - In **parallel**, both functions start simultaneously, results awaited later.
> - Parallel is **faster** because tasks run concurrently.
> - The key difference: `await` each call vs start all, then await.

---

> [!summary] Takeaway
> - **Sequential**: `const r1 = await A(); const r2 = await B();` — slower but ordered.
> - **Parallel**: `const t1 = A(); const t2 = B(); const r1 = await t1; const r2 = await t2;` — faster.
> - Both return the same result but with different execution times.
> - Understand when to use each based on **dependency** between tasks.

---

### 📎 Reference

[Run functions in parallel and in sequence - LearnersBucket](https://learnersbucket.com/examples/interview/run-functions-in-parallel-and-in-sequence-in-javascript/)

---
