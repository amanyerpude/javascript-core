---

> [!quote] Metadata  
> **Posted on:** September 20, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, Javascript  
> **Tags:** #function

---

# Method chaining in JavaScript – Part 2

Showcase a working demo of **method chaining** in JavaScript by implementing a compute amount calculator that handles different currency denominations.

---

> [!example] Example
> 
> ```javascript
> computeAmount().lacs(15).crore(5).crore(2).lacs(20).thousand(45).crore(7).value();
> // Output: 143545000
> ```

---

## Concept

We have already seen an example of method chaining in which we used object-based and function-based implementations. Similarly, we will solve this problem with **different approaches**:

1. **Using function as constructor** - Creates new instances with `new` keyword
2. **Using function as closure** - Returns a new object from closure

The calculator handles different currency denominations:
- **crore**: 10^7 (10,000,000)
- **lacs**: 10^5 (100,000) 
- **thousand**: 10^3 (1,000)
- **hundred**: 10^2 (100)
- **ten**: 10^1 (10)
- **unit**: 10^0 (1)

---

## Implementation

### Method 1: Using Function as Constructor

We will create a constructor function that will help us to create a new instance every time and perform the operations.

```javascript
const ComputeAmount = function(){
  this.store = 0;
  
  this.crore = function(val){
    this.store += val * Math.pow(10, 7);
    return this;
  };
  
  this.lacs = function(val){
    this.store += val * Math.pow(10, 5);
    return this;
  }
  
  this.thousand = function(val){
    this.store += val * Math.pow(10, 3);
    return this;
  }
  
  this.hundred = function(val){
    this.store += val * Math.pow(10, 2);
    return this;
  }
  
  this.ten = function(val){
    this.store += val * 10;
    return this;
  }
  
  this.unit = function(val){
    this.store += val;
    return this;
  }
  
  this.value = function(){
    return this.store;
  }
}
```

### Method 2: Using Function as Closure

We will form closure and return a new object from it that will have all the logic encapsulated.

```javascript
const ComputeAmount = function(){
  
  return {
    store: 0,
    crore: function(val){
      this.store += val * Math.pow(10, 7);
      return this;
    },

    lacs: function(val){
      this.store += val * Math.pow(10, 5);
      return this;
    },

    thousand: function(val){
      this.store += val * Math.pow(10, 3);
      return this;
    },

    hundred: function(val){
      this.store += val * Math.pow(10, 2);
      return this;
    },

    ten: function(val){
      this.store += val * 10;
      return this;
    },

    unit: function(val){
      this.store += val;
      return this;
    },

    value: function(){
      return this.store;
    }
  }
}
```

---

## Input / Output

### Method 1: Constructor Function

```javascript
const computeAmount = new ComputeAmount();
const amount = computeAmount.lacs(15).crore(5).crore(2).lacs(20).thousand(45).crore(7).value();
console.log(amount === 143545000);
```

**Output:**
```javascript
true
```

### Method 2: Closure Function

```javascript
const amount = ComputeAmount().lacs(9).lacs(1).thousand(10).ten(1).unit(1).value();
console.log(amount === 1010011);

const amount2 = ComputeAmount().lacs(15).crore(5).crore(2).lacs(20).thousand(45).crore(7).value();
console.log(amount2 === 143545000);
```

**Output:**
```javascript
true
true
```

---

> [!tip] Key Insight
> 
> - **Constructor approach**: Use `new` keyword to create instances, methods attached to `this`.
>     
> - **Closure approach**: Return object from function, no `new` keyword needed.
>     
> - Both approaches use **`Math.pow()`** for currency conversion calculations.
>     
> - Each method **returns `this`** to enable method chaining.
>     
> - **Closure approach** is more flexible as it doesn't require `new` keyword.

---

> [!summary] Takeaway  
> Method chaining can be implemented using constructor functions or closures, with closures offering more flexibility by eliminating the need for the `new` keyword while maintaining the same chaining behavior.

---

### 🔗 Related Resources

[Read Article on LearnersBucket](https://learnersbucket.com/examples/interview/method-chaining-in-javascript-part-2/)

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/aAJ_P8-84ac?si=teVPZ-9NzEVENgxO)

---
