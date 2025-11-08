# Exercise: Event Loop and Microtasks


> Tags: JavaScript Event Loop Execution, JavaScript Under the Hood Working Mechanism


**Question:**  
Predict the output and explain the sequence: 

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

**Answer:**

```
A 
D 
C 
B 
```

**Why It Works:**  

- A and D run first → synchronous 
- Promise.then (microtask) runs before any setTimeout (macrotask) 
- setTimeout callback runs last 


### 💡 Core Concept: 

> JS has a single-threaded event loop. Microtasks (Promises, queueMicrotask) always precede macrotasks (setTimeout, setInterval) after the current call stack.


### ✅ Event Loop Execution Cycle:
> After the current call stack empties, the event loop first drains the microtask queue completely, then executes one macrotask, and then again processes all pending microtasks before the next macrotask.