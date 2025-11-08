# Exercise: Implement Rate Limiter Function 


> Tags: Closures, Timing Logic, State Management 


**Goal:**  
Implement a function createRateLimiter(limit, interval) that returns another function.
That returned function can be called any number of times, but only up to limit times within every interval milliseconds.


**Example Usage:**  

```js
const limiter = createRateLimiter(3, 5000); // 3 calls every 5s

limiter("A"); // executes
limiter("B"); // executes
limiter("C"); // executes
limiter("D"); // ignored (limit exceeded)
setTimeout(() => limiter("E"), 6000); // executes again (new window)
```


**My Final Code:**  

```js
const rateLimiter = (limit = 30, timeInMs = 1000) => {
    
    const entries = []; 
    
    const rateLimitedFunc = (...args) => {
        
        const now = Date.now();
        let cleaningStaleEntries = true;
        
        while (entries.length && cleaningStaleEntries) {
            
            const earliestEntry = entries[0];
            const slidingWindowEdge = now - timeInMs;
        
            const isStaleEntry = (earliestEntry < slidingWindowEdge);
            
            if (isStaleEntry) {
                entries.shift();
            }
            else cleaningStaleEntries = false;
        }
        
        const isLimitNotExceeded = entries.length < limit;
        
        if (isLimitExceeded) {
            entries.push(now);
            console.log('Running Logic...');
        }
        else console.log('Debounced');
    }
    
    return rateLimitedFunc;
}

const rateLimitedFunc = rateLimiter(3, 500);

setTimeout(() => rateLimitedFunc(), 100);
setTimeout(() => rateLimitedFunc(), 200);
setTimeout(() => rateLimitedFunc(), 200);
setTimeout(() => rateLimitedFunc(), 300);
setTimeout(() => rateLimitedFunc(), 500);
setTimeout(() => rateLimitedFunc(), 601);
setTimeout(() => rateLimitedFunc(), 1000);
```