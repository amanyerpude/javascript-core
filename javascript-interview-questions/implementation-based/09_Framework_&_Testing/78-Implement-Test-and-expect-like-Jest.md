
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** Medium  
> **Posted in:** Interview, JavaScript, Testing  
> **Tags:** #interview #javascript #jest #testing

---

# Implement Test() and expect() like Jest

Write a function to implement `test()` and `expect()` as in Jest from scratch.

---

## Example

```javascript
test("adds 1 + 2 to equal 3", () => {
  expect(1 + 2).toBe(3);
});

test("multiplies 2 * 3 to equal 6", () => {
  expect(2 * 3).toBe(6);
});

// Output:
// ✓ adds 1 + 2 to equal 3 (1 ms)
// ✓ multiplies 2 * 3 to equal 6 (1 ms)
```

---

## Implementation

```javascript
function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`Expected ${expected} but got ${actual}`);
      }
      return true;
    },
    toEqual(expected) {
      if (JSON.stringify(actual) !== JSON.stringify(expected)) {
        throw new Error(
          `Expected ${JSON.stringify(expected)} but got ${JSON.stringify(actual)}`
        );
      }
      return true;
    },
    toBeTruthy() {
      if (!actual) {
        throw new Error(`Expected truthy value but got ${actual}`);
      }
      return true;
    },
    toBeFalsy() {
      if (actual) {
        throw new Error(`Expected falsy value but got ${actual}`);
      }
      return true;
    },
    toBeNull() {
      if (actual !== null) {
        throw new Error(`Expected null but got ${actual}`);
      }
      return true;
    },
    toBeInstanceOf(type) {
      if (!(actual instanceof type)) {
        throw new Error(
          `Expected instance of ${type.name} but got ${typeof actual}`
        );
      }
      return true;
    },
  };
}

function test(name, fn) {
  try {
    fn();
    console.log(`✓ ${name}`);
  } catch (error) {
    console.error(`✗ ${name}`);
    console.error(`  ${error.message}`);
  }
}
```

---

## Extended Matchers

```javascript
function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`Expected ${expected} but got ${actual}`);
      }
    },
    toBeGreaterThan(expected) {
      if (actual <= expected) {
        throw new Error(`Expected ${actual} to be greater than ${expected}`);
      }
    },
    toContain(item) {
      if (!actual.includes(item)) {
        throw new Error(`Expected ${actual} to contain ${item}`);
      }
    },
    toThrow() {
      if (typeof actual !== 'function') {
        throw new Error('Expected a function');
      }
      try {
        actual();
        throw new Error('Expected function to throw');
      } catch (e) {
        if (e.message === 'Expected function to throw') throw e;
      }
    },
  };
}
```

---

> [!tip] Key Points
> - `expect` returns an object with **matcher methods**.
> - Each matcher **throws** on failure.
> - `test` wraps the function in a **try/catch** to report results.
> - Can be extended with more matchers like `toEqual`, `toContain`, etc.

---

> [!summary] Takeaway
> - `expect(actual)` returns an object with chainable **matchers**.
> - `test(name, fn)` runs the test and **reports** pass/fail.
> - Matchers throw **errors** on failure for test reporting.
> - Easy to extend with additional matchers.

---

### 📎 Reference

[You've Used Jest. Now Let's Build It From Scratch - Medium](https://medium.com/@yash-nandvana/youve-used-jest-now-let-s-build-it-from-scratch-4cb4651fc93e)

---
