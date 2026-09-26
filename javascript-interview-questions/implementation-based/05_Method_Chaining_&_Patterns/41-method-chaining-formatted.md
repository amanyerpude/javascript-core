
---

> [!quote] Metadata  
> **Posted on:** February 25, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, Javascript  
> **Tags:** #function

---

# Method chaining in JavaScript

Explain **method chaining** in JavaScript by implementing a calculator that performs the basic actions like add, subtract, divide, and multiply.

---

> [!example] Example
> 
> ```javascript
> calculator.add(10).subtract(2).divide(2).multiply(5);
> console.log(calculator.total);
> // 20
> ```

---

## Concept

Method chaining is an **object-oriented paradigm**, in which the methods usually share the same reference, which in JavaScript is done by sharing `this` (current context) from each method.

We are going to see the **two different implementations** of method chaining:
1. Using objects
2. With functions

---

## Implementation

### Using Objects

Methods inside the object can refer to the current object using `this` keyword, thus we can use the same to perform our operations and return them so that they can be shared around the chain.

```javascript
const calculator = {
  total: 0,
  add: function(val){
    this.total += val;
    return this;
  },
  subtract: function(val){
    this.total -= val;
    return this;
  },
  divide: function(val){
    this.total /= val;
    return this;
  },
  multiply: function(val){
    this.total *= val;
    return this;
  }
};
```

### Method Chaining with Functions

The problem with objects is that we cannot create a new instance of them. But it can be solved using functions.

```javascript
const CALC = function(){
  this.total = 0;

  this.add = (val) => {
    this.total += val;
    return this;
  }

  this.subtract = (val) => {
    this.total -= val;
    return this;
  }

  this.multiply = (val) => {
    this.total *= val;
    return this;
  }

  this.divide = (val) => {
    this.total /= val;
    return this;
  }

  this.value = () => this.total;
}
```

---

## Input / Output

### Object Implementation

```javascript
calculator.add(10).subtract(2).divide(2).multiply(5);
console.log(calculator.total);
```

**Output:**
```javascript
20
```

### Function Implementation

```javascript
const calculator = new CALC();
calculator.add(10).subtract(2).divide(2).multiply(5);
console.log(calculator.total);
```

**Output:**
```javascript
20
```

---

> [!tip] Key Insight
> 
> - Method chaining = **return `this`** from each method to enable chaining.
>     
> - Each method modifies the object state and returns the same object reference.
>     
> - Achieved using **`this` keyword** + **returning `this`** from each method.
>     
> - Functions allow **creating multiple instances**, objects don't.

---

> [!summary] Takeaway  
> Method chaining improves code readability by allowing multiple operations to be performed in a single statement, achieved by returning `this` from each method to maintain the object context.

---

### 🔗 Related Article

[Read on LearnersBucket](https://learnersbucket.com/examples/interview/method-chaining-in-javascript/)

---
