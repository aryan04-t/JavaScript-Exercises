# Exercise: Deep Comparison Between Two Objects 


**Goal:**  
Write a function deepEqual(obj1, obj2) that returns true if both objects are deeply equal, and false otherwise.
You cannot use JSON.stringify for comparison.

**Example Input:**
```js
const a = { name: "Aryan", stats: { height: 186, weight: 78.8 } };
const b = { name: "Aryan", stats: { height: 186, weight: 78.8 } };

deepEqual(a, b); // true
```

**Example Edge Case:**
```js 
const a2 = { a: 1, b: [1,2] };
const b2 = { a: 1, b: [1,2,3] };

deepEqual(a2, b2); // false
```

**My Final Code:**
```js
// Even arrays are checked by this function 
function checkObjectsEqual(a, b) {

    const aKeys = Object.keys(a);
    const bKeys = Object.keys(b);
    
    if (aKeys.length !== bKeys.length) return false;
    
    let isEqual = true;
    
    for (let key of aKeys) {
        
        if (!b.hasOwnProperty(key)) {
            isEqual = false;
            break;
        }
        
        const currEqual = deepEqual(a[key], b[key]);
        
        isEqual = isEqual && currEqual;
        if (!isEqual) break;
    }
    
    return isEqual;
}

// This function checks whether 2 objects are deeply equal or not by value 
// The nested parts can have values: JS objects, arrays, null, undefined, NaN or primitives 
// But this function doesn't check deep equality for complex data types like Map, Set, Functions, etc. 

function deepEqual (a, b) {
    
    if (a === null || b === null) return a === b; // edge case
    
    if (
        (typeof(a) !== 'object' && typeof(b) === 'object') || 
        (typeof(a) === 'object' && typeof(b) !== 'object')
    ) {
        return false;
    }
    
    if (typeof(a) === 'object' && typeof(b) === 'object') {
        return checkObjectsEqual(a, b);
    }
    
    if (Number.isNaN(a) && Number.isNaN(b)) return true; // edge case
    
    return a === b;
}

/*
Have to keep in mind the flaws of JS for handling edge cases, like typeof NaN is 'number', and typeof null is 'object' 
*/


const a1 = { name: "Aryan", stats: { height: 186, weight: 78.8 } };
const b1 = { name: "Aryan", stats: { height: 186, weight: 78.8 } };

console.log(deepEqual(a1, b1)); // true


const a2 = { a: 1, b: [1,2] };
const b2 = { a: 1, b: [1,2,3] };

console.log(deepEqual(a2, b2)); // false
```