
---

> [!quote] Metadata  
> **Posted on:** April 2, 2024  
> **Author:**   
> **Posted in:** Interview, Javascript  
> **Tags:** #polyfill #inheritance #prototype

---

# Polyfill for `extend` in JavaScript

Write a simple **polyfill** for the `extend` method. The goal is to accept two functions — `Parent` and `Child` — and extend the `Child` so that it inherits all of the parent’s **properties** (both static and non-static).

---

## Problem Statement

- Extend one constructor function (`Child`) from another (`Parent`).
    
- Copy:
    
    - **Non-static methods** from the parent’s prototype.
        
    - **Static methods** from the parent function itself.
        
- Ensure the prototype chain is updated so `instanceof` checks work correctly.
    

---

> [!example] Example
> 
> ```javascript
> function Parent() {
>   this.name = "abc";
> }
> 
> Parent.prototype.walk = function(){
>   console.log(this.name + ', I am walking!');
> };
> 
> function Child() {
>   this.name = "pqr";
> }
> 
> Child.prototype.sayHello = function(){
>   console.log('hi, I am a student');
> };
> 
> // extend
> extend(Parent, Child);
> 
> const child = new Child();
> child.sayHello();
> child.walk();
> 
> console.log(child instanceof Parent);
> console.log(child instanceof Child);
> ```
> 
> **Output:**
> 
> ```javascript
> "hi, I am a student"
> "pqr, I am walking!"
> true
> true
> ```

---

## Implementation

```javascript
const myExtends = (SuperType, SubType) => {
  // link child prototype to parent prototype (non-static methods)
  SubType.prototype.__proto__ = SuperType.prototype;
  // ES5 alternative:
  // Object.setPrototypeOf(SubType.prototype, SuperType.prototype);

  // link static methods
  SubType.__proto__ = SuperType;
  // ES5 alternative:
  // Object.setPrototypeOf(SubType, SuperType);

  // ensure child constructor points back to itself
  SubType.prototype.constructor = SubType;
};
```

---

## Full Example

```javascript
// Parent
function Person() {
  this.name = "abc";
}

// non-static methods
Person.prototype.walk = function() {
  console.log(this.name + ', I am walking!');
};

Person.prototype.sayHello = function() {
  console.log('hello');
};

// static method
Person.staticSuper = function() {
  console.log('static');
};

// Child
function Student() {
  this.name = "pqr";
}

// override sayHello
Student.prototype.sayHello = function() {
  console.log('hi, I am a student');
};

// add new method
Student.prototype.sayGoodBye = function() {
  console.log('goodBye');
};

// extend Student from Person
myExtends(Person, Student);

const student1 = new Student();
student1.sayHello();     // hi, I am a student
student1.walk();         // pqr, I am walking!
student1.sayGoodBye();   // goodBye
Student.staticSuper();   // static
console.log(student1.name); // pqr

// inheritance checks
console.log(student1 instanceof Person);  // true
console.log(student1 instanceof Student); // true
```

---

> [!tip] Key Insights
> 
> - `__proto__` (or `Object.setPrototypeOf`) links prototypes for inheritance.
>     
> - Must reset the `constructor` property on the child after linking.
>     
> - Copies **both static and non-static** methods to ensure full inheritance.
>     
> - Works like ES6 `class extends`, but done manually with prototypes.
>     

---
