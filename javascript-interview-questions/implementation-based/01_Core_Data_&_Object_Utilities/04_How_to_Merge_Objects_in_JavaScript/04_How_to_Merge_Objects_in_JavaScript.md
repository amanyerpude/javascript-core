
---

> [!quote] Metadata  
> **Posted on:** June 6, 2019  
> **Author:**   
> **Posted in:** Interview, JavaScript  
> **Tags:** #object #merge #shallow #deep #spread #assign #polyfill

---

# How to Merge Objects in JavaScript

Learn how to merge **two or more objects** in JavaScript.

Almost everything in JavaScript is an object, yet surprisingly, JS didn’t originally have built-in methods for merging them.

With **ES6 and beyond**, new tools make merging easier — but understanding both **native methods** and **custom implementations** is crucial, especially for interviews.

---

## Types of Merges

There are two main strategies:

1. **Shallow Merge** – merges only own properties (doesn’t deeply traverse nested objects).
    
2. **Deep Merge** – merges recursively, combining nested objects and arrays.
    

---

## Shallow Merge Objects

### Using the Spread Operator (`...`)

The spread operator copies all enumerable own properties of source objects into a new object.

```javascript
let obj1 = { name: 'prashant', age: 23 };
let obj2 = { qualification: 'BSC CS', loves: 'Javascript' };

let merge = { ...obj1, ...obj2 };

console.log(merge);
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  qualification: "BSC CS"
}
*/
```

---

### Using `Object.assign()`

`Object.assign(target, ...sources)` copies enumerable own properties from one or more source objects to a target.

```javascript
let obj1 = { name: 'prashant', age: 23 };
let obj2 = { qualification: 'BSC CS', loves: 'Javascript' };

let merge = Object.assign({}, obj1, obj2);

console.log(merge);
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  qualification: "BSC CS"
}
*/
```

> 💡 Passing `{}` as the first argument ensures you don’t mutate the original objects.

---

### Using a Custom Function

You can implement your own shallow merge function:

```javascript
let merge = (...arguments) => {
  let target = {};

  let merger = (obj) => {
    for (let prop in obj) {
      if (obj.hasOwnProperty(prop)) {
        target[prop] = obj[prop];
      }
    }
  };

  for (let i = 0; i < arguments.length; i++) {
    merger(arguments[i]);
  }

  return target;
};
```

**Demo:**

```javascript
let obj1 = { name: 'prashant', age: 23 };
let obj2 = { qualification: 'BSC CS', loves: 'Javascript' };

let merged = merge(obj1, obj2);

console.log(merged);
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  qualification: "BSC CS"
}
*/
```

---

## Deep Merge Objects

A deep merge copies **nested objects** as well, not just top-level properties.

The best approach is a **recursive function**.

```javascript
let merge = (...arguments) => {
  let target = {};

  let merger = (obj) => {
    for (let prop in obj) {
      if (obj.hasOwnProperty(prop)) {
        if (Object.prototype.toString.call(obj[prop]) === '[object Object]') {
          target[prop] = merge(target[prop], obj[prop]);
        } else {
          target[prop] = obj[prop];
        }
      }
    }
  };

  for (let i = 0; i < arguments.length; i++) {
    merger(arguments[i]);
  }

  return target;
};
```

**Demo:**

```javascript
let obj1 = {
  name: 'prashant',
  age: 23,
  nature: { "helping": true, "shy": false }
};

let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript',
  nature: { "angry": false, "shy": true }
};

console.log(merge(obj1, obj2));
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  nature: Object {
    angry: false,
    helping: true,
    shy: true
  },
  qualification: "BSC CS"
}
*/
```

---

## Unified Function (Shallow + Deep)

We can combine both shallow and deep merge into a **single function**.  
If the first argument is `true`, it performs a **deep merge**. Otherwise, it does a **shallow merge**.

```javascript
let merge = (...arguments) => {
   let target = {};
   let deep = false;
   let i = 0;

   if (typeof (arguments[0]) === 'boolean') {
      deep = arguments[0];
      i++;
   }

   let merger = (obj) => {
    for (let prop in obj) {
      if (obj.hasOwnProperty(prop)) {
        if (deep && Object.prototype.toString.call(obj[prop]) === '[object Object]') {
          target[prop] = merge(target[prop], obj[prop]);
        } else {
          target[prop] = obj[prop];
        }
      }
    }
   };

   for (; i < arguments.length; i++) {
    merger(arguments[i]);
   }

   return target;
};
```

**Demo:**

```javascript
let obj1 = {
  name: 'prashant',
  age: 23,
  nature: { "helping": true, "shy": false }
};

let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript',
  nature: { "angry": false, "shy": true }
};

// Shallow merge
console.log(merge(obj1, obj2));
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  nature: Object {
    angry: false,
    shy: true
  },
  qualification: "BSC CS"
}
*/

// Deep merge
console.log(merge(true, obj1, obj2));
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  nature: Object {
    angry: false,
    helping: true,
    shy: true
  },
  qualification: "BSC CS"
}
*/
```

---

## Other Libraries

Sometimes it’s easier to rely on tested libraries:

- **jQuery `$.extend()`**
    
    ```javascript
    $.extend(true, {}, obj1, obj2);
    ```
    
    Performs deep or shallow copy based on the first boolean argument.
    
- **Lodash `_.merge()`**  
    A utility that deep merges objects and arrays recursively.
    

---

> [!tip] Key Insights
> 
> - **Spread (`...`) and `Object.assign`** are great for quick shallow merges.
>     
> - Use **custom recursion** for deep merging without external libraries.
>     
> - **jQuery and Lodash** provide ready-made, reliable solutions for production.
>     
> - Unified merge functions (shallow + deep) are useful in interview scenarios.
>     

---

> [!summary] Takeaway  
> Merging objects is a **common real-world need** — from config handling to API responses.
> 
> - Shallow merge → fast but limited.
>     
> - Deep merge → powerful, but requires recursion or libraries.
>     
> 
> Knowing both approaches (and when to use them) is an **essential JavaScript skill**.

---

### 📎 Reference

[Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/how-to-merge-objects-in-javascript/)

---
