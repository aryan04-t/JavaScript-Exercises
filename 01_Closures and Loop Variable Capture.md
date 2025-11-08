# Exercise: Closures and Loop Variable Capture


**Question:**  
What will be the output of the following code and why?


```js
function createCounters() {
  const counters = [];
  for (var i = 0; i < 3; i++) {
    counters.push(function() {
      return i;
    });
  }
  return counters;
}

const counters = createCounters(); 

console.log(counters[0]());
console.log(counters[1]());
console.log(counters[2]());
```


**Follow-up:**  
How can you fix it so that it prints `0 1 2`?


**Answer:**  

- The variable i is declared with var, which is function-scoped, not block-scoped. 
- During the loop, each pushed function closes over the same i variable because due to `var` declartion i variable is scoped at the function level, and that's why the each pushed function is not closing over i variable's value at that time. 
- When the loop ends, i becomes 3, and all three functions reference this same variable — hence all return 3. 

**Fix #1 — Use let (Block Scope Binding):**  
```js 
for (let i = 0; i < 3; i++) { ... } // 'let' is block-scoped
```

`Why it works:`
let is block-scoped — each iteration creates a new lexical environment with its own copy of i.
Thus, each closure “remembers” the distinct i value from its own iteration.


**Fix #2 — Use an IIFE (Classic Closure Pattern)**  

```js
function createCounters() {
  const counters = [];
  for (var i = 0; i < 3; i++) {
    (function(x) {
      counters.push(function() {
        return x;
      });
    })(i);
  }
  return counters;
}
```

`Why it works:`
Each IIFE immediately invokes with the current i as x, creating a new function scope that captures x by value.
Thus, every pushed function has its own independent copy of x.


- - - 


# Closures:   
> A closure is a function that "remembers" and can access variables from its lexical scope, even after that scope has finished executing.


**💡 Simpler Explanation**  

> When a function is defined inside another function, the inner function forms a closure — it closes over the outer function’s variables.
> This means the inner function retains access to those variables, even after the outer function has returned.


**Closure Example:**  
```js
function outer() {
  let count = 0;

  function inner() {
    count++;
    return count;
  }

  return inner;
}

const counter = outer(); // outer() has finished, but 'count' still exists!
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```


**✅ Why this works:**  

- outer() returns the function inner.
- Even though outer() has finished executing, the variable count is kept alive by the closure formed by inner.
- Each call to counter() accesses and updates that preserved count variable.


**⚙️ In Other Words**

`A closure = function + its lexical environment (the scope in which variable is created/maintained).`