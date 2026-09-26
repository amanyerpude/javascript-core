
---

> [!quote] Metadata  
> **Posted on:** April 2, 2024  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #immutability #helper #function

---

# Create an Immutability Helper in JavaScript

Implement a simple immutability helper in JavaScript that allows a certain set of actions to update the frozen input object. The input object can only be updated through this function, and the returned value is also **frozen**.

⚠️ **Note:** For simplicity, only one operation is allowed at a time.

---

## Supported Actions

### 🔹 `_push_` : Array

Push values into the destination array in the input array.

```javascript
const inputArr = [1, 2, 3, 4];
const outputArr = update(
  inputArr,  {_push_: [5, 6, 7]}
);

console.log(outputArr);
// [1, 2, 3, 4, 5, 6, 7]
```

---

### 🔹 `_replace_` : Object | Array

Replace the destination value in the input object/array.

**Object Example:**

```javascript
const state = {
  a: { b: { c: 1 } },
  d: 2
};

const newState = update(
  state, 
  {a: {b: { c: {_replace_: 3}}}}
);

console.log(newState);
/*
{
  "a": { "b": { "c": 3 } },
  "d": 2
}
*/
```

**Array Example:**

```javascript
const inputArr = [1, 2, 3, 4];
const outputArr = update(
  inputArr, 
  {1: {_replace_: 10}}
);

console.log(outputArr);
// [1, 10, 3, 4]
```

---

### 🔹 `_merge_` : Object

Merge the destination value in the input object.

```javascript
const state = {
  a: { b: { c: 1 } },
  d: 2
};

const newState = update(
  state, 
  {a: {b: { _merge_ : {e: 5 }}}}
);

console.log(newState);
/*
{
  "a": { "b": { "c": 1, "e": 5 } },
  "d": 2
}
*/
```

---

### 🔹 `_transform_` : Object | Array

Transform the destination value by passing it through a function.

```javascript
const input = {a: { b: 2}};
const output = update(
  input, {a: { b: {_transform_: (item) => item * 2}}}
);

console.log(output);
/*
{
  "a": { "b": 4 }
}
*/
```

---

## Implementation

```javascript
// deepFreeze utility
function deepFreeze(object) {
  var propNames = Object.getOwnPropertyNames(object);

  for (let name of propNames) {
    let value = object[name];
    object[name] = value && typeof value === "object" 
      ? deepFreeze(value) 
      : value;
  }

  return Object.freeze(object);
}

// update helper
function update(inputObj, action) {
  const clone = JSON.parse(JSON.stringify(inputObj));
  
  function helper(target, action) {
    for (const [key, value] of Object.entries(action)) {
      switch (key) {
        case '_push_':
          return [...target, ...value];

        case '_replace_':
          return value;

        case '_merge_':
          if (!(target instanceof Object)) {
            throw Error("bad merge");
          }
          return {...target, ...value};

        case '_transform_':
          return value(target);

        default:
          if (target instanceof Array) {
            const res = [...target];
            res[key] = update(target[key], value);
            return res;
          } else {
            return {
              ...target,
              [key]: update(target[key], value)
            };
          }
      }
    }
  }
  
  const output = helper(clone, action);
  deepFreeze(output);
  return output;
}
```

---

## Test Cases

### ✅ Test Case 1: `_push_`

```javascript
const inputArr = [1, 2, 3, 4];
const outputArr = update(inputArr, {_push_: [5, 6, 7]});
console.log(outputArr);
// [1,2,3,4,5,6,7]
```

---

### ✅ Test Case 2: `_replace_`

```javascript
const state = { a: { b: { c: 1 } }, d: 2 };
deepFreeze(state);

const newState = update(state, {a: {b: { c: {_replace_: 3}}}});
newState.a.b.c = 10; // ❌ no effect (frozen)

console.log(newState);
/*
{
  "a": { "b": { "c": 3 } },
  "d": 2
}
*/
```

---

### ✅ Test Case 3: `_merge_`

```javascript
const state = { a: { b: { c: 1 } }, d: 2 };
deepFreeze(state);

const newState = update(state, {a: {b: {_merge_: {e: 5}}}});
newState.a.b.e = 10; // ❌ no effect (frozen)

console.log(newState);
/*
{
  "a": { "b": { "c": 1, "e": 5 } },
  "d": 2
}
*/
```

---

### ✅ Test Case 4: `_transform_`

```javascript
const state = {a: { b: 2}};
deepFreeze(state);

const newState = update(state, {a: { b: {_transform_: (item) => item * 2}}});
newState.a.b = 10; // ❌ no effect (frozen)

console.log(newState);
/*
{
  "a": { "b": 4 }
}
*/
```

---

> [!tip] Key Insights
> 
> - Objects are **deep frozen** after update, preventing further modifications.
>     
> - Each action (`_push_`, `_replace_`, `_merge_`, `_transform_`) is handled recursively.
>     
> - Only one action is supported at a time for simplicity.
>     

---
