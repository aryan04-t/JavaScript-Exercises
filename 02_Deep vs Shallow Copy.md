# Exercise: Deep vs Shallow Copy


> Tags: JSON, JavaScript Object Copying, Deep & Shallow Copy Logics, Object Value - References - and Memory Addresses  


**Question:**  
What’s the difference between shallow and deep copies in JS objects, and how do you perform each?  
Also — predict the output below:

```js
const user = {
  name: "Aryan",
  details: { age: 21, city: "Delhi" },
};

const copy1 = { ...user };
const copy2 = JSON.parse(JSON.stringify(user));

copy1.details.city = "Mumbai";
copy2.details.age = 22;

console.log(user.details.city); // ❓What will this log? 
console.log(user.details.age);  // ❓And this? 
console.log(copy1 === user);    // ❓
console.log(copy1.details === user.details); // ❓
```

### 🔹 Step-by-step reasoning of each line of code affects:

**1.** Original user:

```js
{
  name: "Aryan",
  details: { age: 21, city: "Delhi" }
}
```

**2.** copy1 = { ...user }

- This performs a shallow copy.
- Meaning: top-level properties are copied, but nested objects (like details) are shared by reference.

- So now:
```js
copy1.details === user.details  ✅ (same reference)
```

**3.** This performs a deep copy by serializing and deserializing the object.

```js 
copy2 = JSON.parse(JSON.stringify(user)) 
```

- The new object `copy2` has its own copy of details.
- So:
```js 
copy2.details !== user.details  ✅ (different reference)
```

**4.** Since copy1.details and user.details point to the same object in memory,

```js
copy1.details.city = "Mumbai"
```
- this mutates the same object.

- Thus: `user.details.city -> "Mumbai"`

**5.** copy2.details is not the same object, so this change affects only copy2, not user.
```js
copy2.details.age = 22
```


### Output:  

```js
console.log(user.details.city); // "Mumbai"
console.log(user.details.age);  // 21
console.log(copy1 === user);    // false
console.log(copy1.details === user.details); // true
```

### 🔹 Output Explanation Summary:

| Expression | Output | Why |
|-------------|---------|-----|
| `user.details.city` | `"Mumbai"` | Shallow copy → same nested reference as `user` |
| `user.details.age` | `21` | `copy2` is deep cloned → independent from `user` |
| `copy1 === user` | `false` | Different top-level object, both have their own different memory address |
| `copy1.details === user.details` | `true` | Shared reference due to shallow copy |


### 💡 Key Takeaways: 

- Shallow copy ({...obj}, Object.assign()) → copies top-level only by value, all nested reference objects are still copied by reference. 

- Deep copy (JSON.parse(JSON.stringify(obj)), structuredClone(obj)) → recursively copies nested objects by value.

- structuredClone(obj) is now a js native function, also faster and more safe then the older JSON stringify-parsing method for creating deep copies, but even structuredClone cannot clone non-serializable data (like functions, DOM nodes, etc.). 