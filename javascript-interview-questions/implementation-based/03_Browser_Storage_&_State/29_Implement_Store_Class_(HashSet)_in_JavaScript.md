
---

> [!quote] Metadata  
> **Posted on:** April 27, 2022  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #hashset #store #function

---

# Implement Store Class (HashSet) in JavaScript

Implement a simple store class (hashSet) in JavaScript with `set(key, value)`, `get(key)`, & `has(key)` methods.

---

## Problem Statement

Create a function/class that will:

- Store a list of key-value pairs.
    
- Expose a method to check if the key exists in the store.
    
- Expose a method to get the value associated with the key.
    
- Expose a method to store the key-values.
    

---

> [!example] Example
> 
> ```javascript
> const store = new Store();
> 
> store.set('a', 10);
> store.set('b', 20);
> store.set('c', 30);
> 
> store.get('b'); // 20
> store.has('c'); // true
> ```
> 
> **Output:**
> 
> ```javascript
> 20
> true
> ```

---

## Approach

Reading the problem statement we can derive that we have to create a simple function that will store the list of key-value pairs and will expose two methods, one to check if the key exists in the store and the second to get the value associated with the key, apart from these one more method to store the key-values.

We can do this by simply creating a function with an object that will store the key-value and these methods.

---

## Implementation

```javascript
const Store = function(){
  // store the data
  this.list = {};
  
  // set the key-value in list
  this.set = function(key, value){
    this.list[key] = value;
  };
  
  // get the value of the given key
  this.get = function(key){
    return this.list[key];
  };
  
  // check if key exists
  this.has = function(key){
    return !!this.list[key];
  };
};
```

---

## Input

```javascript
const store = new Store();
store.set('a', 10);
store.set('b', 20);
store.set('c', 30);

console.log(store.get('b'));
console.log(store.has('c'));
console.log(store.get('d'));
console.log(store.has('e'));
```

---

## Output

```javascript
20
true
undefined
false
```

---

> [!tip] Key Insights
> 
> - This mimics a simple **hashSet/hashMap** implementation.
>     
> - `!!this.list[key]` is used to coerce the result into a boolean.
>     
> - Works for learning, but in real projects the built-in `Map` and `Set` are more robust (support objects as keys, iteration, etc.).
>     

---
