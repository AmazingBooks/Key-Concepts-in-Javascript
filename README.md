# Advanced Key-Concepts-in-Javascript
A comprehensive and easy to understand guide for Javascript advanced concepts with examples and details. 
Information is structured and gathered from different resources, mentioned in the Reference part of the book. 

Below is a resume for the most important must known features in ES6 with code examples included: 
  1. ROMISES AND ASYNCHRONOUS PROGRAMMING
  2. PROXIES AND THE REFLECTION API
  3. STRINGS AND REGULAR EXPRESSIONS
  4. BLOCK BINDINGS

## 1. PROMISES and ASYNCHRONOUS PROGRAMMING
### What is a PROMISE? 
A promise is an Object that keeps track whether a certain event has happened already or not and determines what happens after the event ends. A Promise implements the concept of a future value that we’re expecting.
“It’s saying - hey get me some data from the server in the background and it promises that it will get that data”;
In a PROMISE we are dealing with time sensitive code thus a PROMISE can have different states: 
  - **Pending** - before the event has happening, the Promise is pending, 
  - **Settled, Resolved** -  after the event has happen;
  - **Fulfilled** - when the promise what settled, resolved successful;
  - **Rejected** - if there was an error;

In more practical terms we can Produce and Consume Promises; so when we produce a Promise we create a new Promise and send the result using that Promise, than when we consume it, we can use callback functions for fulfillment or rejection of that promises.

![Promise](https://raw.githubusercontent.com/AmazingBooks/Key-Concepts-in-Javascript/master/images/Promise.png)


#### Asynchronous Programming Background

JavaScript engines are built on the concept of a single-threaded event loop. Single-threaded means that only one piece of code is executed at a time. JavaScript engines can execute only one piece of code at a time, so they need to keep track of code that is meant to run. That code is kept in a job queue. Whenever a piece of code is ready to be executed, it is added to the job queue. When the JavaScript engine is finished executing code, the event loop executes the next job in the queue. The event loop is a process inside the JavaScript engine that monitors code execution and manages the job queue. Keep in mind that as a queue, job execution runs from the first job in the queue to the last.

One of the most powerful aspects of JavaScript is how easily it handles Asynchronous Programming. As a language created for the web, JavaScript needed to be able to respond to Asynchronous user interactions, such as clicks and key presses, from the beginning. But as more programs started using asynchronous programming, events and callbacks weren’t powerful enough to support everything developers wanted to do. Promises are the solution to this problem.

JavaScript engines are built on the concept of a _Single-threaded event loop_. Single-threaded means that only one piece of code is executed at a time. Maintaining and protecting state when multiple pieces of code can access and change that state is a difficult problem and a frequent source of bugs in thread-based software.

JavaScript engines can execute only one piece of code at a time, so they need to keep track of code that is meant to run. That code is kept in a job queue. Whenever a piece of code is ready to be executed, it is added to the job queue. When the JavaScript engine is finished executing code, the event loop executes the next job in the queue. The event loop is a process inside the JavaScript engine that monitors code execution and manages the job queue. Keep in mind that as a queue, job execution runs from the first job in the queue to the last.

#### The Event Model

When a user clicks a button or presses a key on the keyboard, an event like onclick is triggered. That event might respond to the interaction by adding a new job to the back of the job queue. This is JavaScript’s most basic form of Asynchronous Programming. The event handler code doesn’t execute until the event fires, and when it does execute, it has the appropriate context. For example:

```javascript
let button = document.getElementById("my-btn");
button.onclick = function(event) {
    console.log("Clicked");
};
```
In this code, console.log("Clicked") will not be executed until button is clicked. When button is clicked, the function assigned to onclick is added to the back of the job queue and will be executed when all other jobs ahead of it are complete.
Events work well for simple interactions, but chaining multiple separate asynchronous calls together is more complicated because you must keep track of the event target (button in this example) for each event. Additionally, you need to ensure that all appropriate event handlers are added before the first time an event occurs. For instance, if button is clicked before onclick is assigned, nothing will happen. So although events are useful for responding to user interactions and similar infrequent functionality, they aren’t very flexible for more complex needs.

#### The Callback Pattern > Callback Hell
The callback pattern is more flexible than events because chaining multiple calls together is easier with callbacks. Here’s an example:

```javascript
readFile("example.txt", function(err, contents) {
if(err){
  throw err;
}
writeFile( 'info.text', if(err) {
  throw err;
})
console.log('File was writtten!');
  )}; 
)};
```
The above pattern works fairly well, but you can quickly find yourself in callback hell. Callback hell occurs when you nest too many callbacks, like this:

```javascript
method1(function(err, result) {

    if (err) {
        throw err;
    }

    method2(function(err, result) {

        if (err) {
            throw err;
        }

        method3(function(err, result) {

            if (err) {
                throw err;
            }

            method4(function(err, result) {

                if (err) {
                    throw err;
                }

                method5(result);
            });

        });

    });

});

```

Nesting multiple method calls, as this example does, creates a tangled web of code that is difficult to understand and debug. 
Callbacks also present problems when you want to implement more complex functionality. 
    - What if you want two asynchronous operations to run in parallel and notify you when they’re both complete? 
    - What if you want to start two asynchronous operations at the same time but only take the result of the first one to complete? 
In these cases, you’d need to track multiple callbacks and cleanup operations, and promises greatly improve such situations.


**The Event Model**
When a user clicks a button or presses a key on the keyboard, an event like onclick is triggered. That event might respond to the interaction by adding a new job to the back of the job queue. This is JavaScript’s most basic form of asynchronous programming. The event handler code doesn’t execute until the event fires, and when it does execute, it has the appropriate context. 
For example:
```javascript
 console.log("Clicked");
};
```
In this code, console.log("Clicked") will not be executed until button is clicked. When button is clicked, the function assigned to onclick is added to the back of the job queue and will be executed when all other jobs ahead of it are complete.
Although events are useful for responding to user interactions and similar infrequent functionality, they aren’t very flexible for more complex needs.

**The Callback Pattern**
The callback pattern is more flexible than events because chaining multiple calls together is easier with callbacks. 
Here’s an example:
```javascript
readFile("example.txt", function(err, contents) {
    if (err) {
        throw err;
    }

    writeFile("example.txt", function(err) {
        if (err) {
            throw err;
        }

        console.log("File was written!");
    });
});
```
This pattern works fairly well, but you can quickly find yourself in callback hell. Callback hell occurs when you nest too many callbacks, like this:
```javascript
method1(function(err, result) {

    if (err) {
        throw err;
    }

    method2(function(err, result) {

        if (err) {
            throw err;
        }

        method3(function(err, result) {

            if (err) {
                throw err;
            }

            method4(function(err, result) {

                if (err) {
                    throw err;
                }

                method5(result);
            });

        });

    });

});
``` 
Callbacks also present problems when you want to implement more complex functionality. What if you want two asynchronous operations to run in parallel and notify you when they’re both complete? What if you want to start two asynchronous operations at the same time but only take the result of the first one to complete? In these cases, you’d need to track multiple callbacks and cleanup operations, and promises greatly improve such situations.


**Promise Basics**
A promise is a placeholder for the result of an asynchronous operation. Instead of subscribing to an event or passing a callback to a function, the function can return a promise, as shown here:

```javascript
// readFile promises to complete at some point in the future
let promise = readFile("example.txt");
``` 
In this code, readFile() doesn’t start reading the file immediately: that will happen later. Instead, the function returns a promise object representing the asynchronous read operation so you can work with it in the future. Exactly when you’ll be able to work with that result depends entirely on how the promise’s life cycle concludes.

**Anatomy of a PROMISE** - ***Promise Life Cycle***
Each promise goes through a short life cycle starting in the pending state, which indicates that the asynchronous operation hasn’t completed yet. 

**How can we check a state of a Promise?** - An internal [[PromiseState]] property is set to "pending", "fulfilled", or "rejected" to reflect the promise’s state. This property isn’t exposed on promise objects, so you can’t determine which state the promise is in programmatically. But you can take a specific action when a promise changes state by using the then() method.  

**then() Method** - The then() method is present on all promises and takes two arguments. 
 1. The first argument is a function to call when the promise is <ins>fulfilled</ins>.
 2. The second argument is a function to call when the promise is rejected.
 
```javascript
let promise = readFile("example.txt");

promise.then(function(contents) {
    // fulfillment
    console.log(contents);
}, function(err) {
    // rejection
    console.error(err.message);
});

promise.then(function(contents) {
    // fulfillment
    console.log(contents);
});

promise.then(null, function(err) {
    // rejection
    console.error(err.message);
});


```
The then() and catch() methods are intended to be used in combination to properly handle the result of asynchronous operations. 
Events tend not to fire when there’s an error, and in callbacks you must always remember to check the error argument.
Just know that if you don’t attach a rejection handler to a promise, all failures will happen silently. Always attach a rejection handler, even if the handler just logs the failure.

> <ins>IMPORTANT</ins>
>  - Any object that implements the then() method as described in the preceding paragraph is called a thenable. All promises are thenables, but all thenables are not promises.
>  - Each call to then() or catch() creates a new job to be executed when the promise is resolved. But these jobs end up in a separate job queue that is reserved strictly for promises
>  

The 3 states of a Promise are:

1. **<ins>Unsettled</ins>** - which is a pending Promise. The promise in the previous example is in the pending state as soon as the readFile() function returns it. 

**How do we create an Unsettled PROMISE?**
New promises are created using the **Promise Constructor**. This constructor accepts a single argument: a function called the **Executor**, which contains the code to initialize the Promise. Two functions are passed as arguments to the **Executor** - resolve() and reject(). 
    - resolve() function is called when the executor has finished successfully to signal that the promise is ready to be resolved,
    - reject() function indicates that the executor has failed.

```javascript
 // Node.js example

let fs = require("fs");

function readFile(filename) {
    return new Promise(function(resolve, reject) {

        // trigger the asynchronous operation
        fs.readFile(filename, { encoding: "utf8" }, function(err, contents) {

            // check for errors
            if (err) {
                reject(err);
                return;
                }

            // the read succeeded
            resolve(contents);

        });
    });
}

  let promise = readFile("example.txt");

  // listen for both fulfillment and rejection
  promise.then(function(contents) {
      // fulfillment
      console.log(contents);
  }, function(err) {
      // rejection
      console.error(err.message);
  });

```
When either resolve() or reject() is called inside the executor, a job is added to the job queue to resolve the promise. This is called job scheduling, and if you’ve ever used the setTimeout() or setInterval() functions, you’re already familiar with it. In job scheduling, you add a new job to the job queue to say, “Don’t execute this right now, but execute it later.”

For instance, the setTimeout() function lets you specify a delay before a job is added to the queue:
```javascript
// add this function to the job queue after 500 ms have passed
//This code schedules a job to be added to the job queue after 500 ms.
setTimeout(function() {
    console.log("Timeout");
}, 500)

console.log("Hi!");
```
Promises work similarly. The promise executor executes immediately, before anything that appears after it in the source code. For instance:
```javascript
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});
console.log("Hi!");
```
Calling resolve() triggers an asynchronous operation. Functions passed to then() and catch() are executed asynchronously, because these are also added to the job queue. Here’s an example:
```javascript
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

promise.then(function() {
    console.log("Resolved.");
});

console.log("Hi!");
```

**Creating Settled Promises**
How do we create Settled Promises? - The Promise constructor is the best way to create unsettled promises due to the dynamic nature of what the promise executor does. But if you want a promise to represent just a single known value doesn’t make sense to schedule a job that simply passes a value to the resolve() function. Instead, you can use either of two methods that create settled promises given a specific value.

***Using Promise.resolve()*** - The Promise.resolve() method accepts a single argument and returns a promise in the fulfilled state.

```javascript
let promise = Promise.resolve(42);
promise.then(function(value) {
    console.log(value);         // 42
});

```

***Using Promise.reject()*** - The Promise.resolve() except the created promise is in the rejected state, as follows:

```javascript
let promise = Promise.reject(42);
promise.then(function(value) {
    console.log(value);         // 42
});

```
##### NOTE: 
If you pass a promise to either the Promise.resolve() or Promise.reject() method, the promise is returned without modification.

****Non-Promise Thenables***
Both Promise.resolve() and Promise.reject() also accept non-promise thenables as arguments. 
When passed a non-promise thenable, these methods create a new promise that is called after the then() function.
A non-promise thenable is created when an object has a then() method that accepts a resolve and a reject argument, like this:

```javascript
let thenable = {
    then: function(resolve, reject) {
        resolve(42);
    }
};
```
Once the asynchronous operation completes, the promise is considered settled and enters one of two possible states:

2. **Fulfilled** - The promise’s asynchronous operation has completed successfully.

3. **Rejected** - The promise’s asynchronous operation didn’t complete successfully due to either an error or some other cause.
One of the most controversial aspects of promises is the silent failure that occurs when a promise is rejected without a rejection handler. Some consider this the biggest flaw in the specification because it’s the only part of the JavaScript language that doesn’t make errors apparent. You can call then() or catch() at any point and have them work correctly regardless of whether the promise is settled or not, making it difficult to know precisely when a promise will be handled. 
Although it’s possible that a future version of ECMAScript will address this problem, both Node.js and browsers have implemented changes to address this developer pain point. They aren’t part of the ECMAScript 6 specification but are valuable tools when you’re using promises.

###### Node.js Rejection Handling

Node.js emits two events on the process object that are related to promise rejection handling:
 - unhandledRejection - Emitted when a promise is rejected and no rejection handler is called within one turn of the event loop.
 
 ```javascript
let rejected;

process.on("unhandledRejection", function(reason, promise) {
    console.log(reason.message);            // "Explosion!"
    console.log(rejected === promise);      // true
});

rejected = Promise.reject(new Error("Explosion!"));

```
 - rejectionHandled - Emitted when a promise is rejected and a rejection handler is called after one turn of the event loop. 
 
  ```javascript
let rejected;

process.on("unhandledRejection", function(reason, promise) {
    console.log(reason.message);            // "Explosion!"
    console.log(rejected === promise);      // true
});

rejected = Promise.reject(new Error("Explosion!"));
```
These events are designed to work together to help identify promises that are rejected and not handled.

###### Browser Rejection Handling
Browsers also emit two events to help identify unhandled rejections. These events are emitted by the window object and are effectively the same as their Node.js equivalents:
 - unhandledrejection -  Emitted when a promise is rejected and no rejection handler is called within one turn of the event loop.
 - rejectionhandled - Emitted when a promise is rejected and a rejection handler is called after one turn of the event loop. 
 
 ### Chaining Promises
Each call to then() or catch() actually creates and returns another promise.
This second promise is resolved only when the first has been fulfilled or rejected. Consider the example below:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});
//The call to p1.then() returns a second promise on which then() is called. The second then() fulfillment handler is only called after the first promise has been resolved. 
p1.then(function(value) {
    console.log(value);
}).then(function() {
    console.log("Finished");
});
```
#### Returning Values in Promise Chains
Another important aspect of promise chains is the ability to pass data from one promise to the next. 

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);         // "42"
    return value + 1;
//The fulfillment handler for p1 returns value + 1 when executed. Because value is 42 (from the executor), 
//    the fulfillment handler returns 43.
}).then(function(value) {
    console.log(value);         // "43"
});
```

#### Returning Promises in Promise Chains
Returning primitive values from fulfillment and rejection handlers allows for the passing of data between promises, but what if you return an object? If the object is a promise, there’s an extra step that’s taken to determine how to proceed. Consider the following example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

p1.then(function(value) {
    // p1 schedules a job that resolves to 42. 
    // first fulfillment handler
    console.log(value);     // 42
    // The fulfillment handler for p1 returns p2, a promise already in the resolved state. 
    return p2;    
}).then(function(value) {
    // second fulfillment handler
    // The second fulfillment handler is called because p2 has been fulfilled. 
    // If p2 were rejected, a rejection handler (if present) would be called instead of the second fulfillment handler.   
    // The important thing to recognize about this pattern is that the second fulfillment handler is not added to p2 but rather to a third promise. 
    console.log(value);     // 43
});
```
#### Responding to Multiple Promises
Each example in this chapter so far has dealt with responding to one promise at a time. But sometimes you’ll want to monitor the progress of multiple promises to determine the next action. 
ECMAScript 6 provides two methods that monitor multiple promises: **Promise.all()** and **Promise.race()**.

##### The Promise.all() Method
Accepts a single argument, which is an iterable (such as an array) of promises to monitor, and returns a promise that is resolved only when every promise in the iterable is resolved. The returned promise is fulfilled when every promise in the iterable is fulfilled, as in this example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.then(function(value) {
    console.log(Array.isArray(value));  // true
    console.log(value[0]);              // 42
    console.log(value[1]);              // 43
    console.log(value[2]);              // 44
});
```

##### The Promise.race() Method
Provides a slightly different take on monitoring multiple promises. This method also accepts an iterable of promises to monitor and returns a promise, but the returned promise is settled as soon as the first promise is settled. Instead of waiting for all promises to be fulfilled, like the Promise.all() method, the Promise.race() method returns an appropriate promise as soon as any promise in the array is fulfilled. For example:

```javascript
let p1 = Promise.resolve(42);

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.race([p1, p2, p3]);

p4.then(function(value) {
    console.log(value);     // 42
});
```

##### Inheriting from Promises
Just like other built-in types, you can use a promise as the base for a derived class. This allows you to define your own variation of promises to extend what built-in promises can do.
For instance, suppose you want to create a promise that can use methods named success() and failure() in addition to the usual then() and catch() methods. You could create that promise type as follows:

```javascript
class MyPromise extends Promise {

    // use default constructor

    success(resolve, reject) {
        return this.then(resolve, reject);
    }

    failure(reject) {
        return this.catch(reject);
    }

}

// MyPromise is derived from Promise and has two additional methods. 
// The success() method mimics resolve() and failure() mimics the reject() method.
let promise = new MyPromise(function(resolve, reject) {
    resolve(42);
});

promise.success(function(value) {
    console.log(value);             // 42
}).failure(function(value) {
    console.log(value);
});


```

### Catching Errors
Promise chaining allows you to catch errors that may occur in a fulfillment or rejection handler from a previous promise. For example:
```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    throw new Error("Boom!");
// In this code, the fulfillment handler for p1 throws an error. 
// The chained call to the catch() method, which is on a second promise, is able to receive that error through its rejection handler. 
}).catch(function(error) {
    console.log(error.message);     // "Boom!"
});
```
Always make sure to have a rejection handler at the end of a promise chain to ensure that you can properly handle any errors that may occur.

### FUTURE ASYNCHRONOUS TASK RUNNING
Bringing a simpler syntax to asynchronous task running in JavaScript is under way. For instance, an await syntax is in progress. 
The basic idea is to use a function marked with async instead of a generator and use await instead of yield when calling a function, such as:

```javascript
(async function() {
    let contents = await readFile("config.json");
    doSomethingWith(contents);
    console.log("Done");
});
```
The async keyword before function indicates that the function is meant to run in an asynchronous manner. The await keyword signals that the function call to readFile("config.json") should return a promise, and if it doesn’t, the response should be wrapped in a promise.
The await syntax is expected to be finalized in ECMAScript 2017 (ECMAScript 8).

### Summary
 1. Promises are designed to improve asynchronous programming in JavaScript by giving you more control and composability over asynchronous operations than events and callbacks can.
 2. Promises schedule jobs to be added to the JavaScript engine’s job queue for future execution, and a second job queue tracks promise fulfillment and rejection handlers to ensure proper execution.
 3. Promises have three states: pending, fulfilled, and rejected. A promise starts in a pending state and becomes fulfilled on a successful execution or rejected on a failure. In either case, you can add handlers to indicate when a promise is settled. The then() method allows you to assign a fulfillment and rejection handler, and the catch() method allows you to assign only a rejection handler.
 4. You can chain promises together in a variety of ways and pass information between them. Each call to then() creates and returns a new promise that is resolved when the previous one is resolved. Such chains can be used to trigger responses to a series of asynchronous events. You can also use Promise.race() and Promise.all() to monitor the progress of multiple promises and respond accordingly.
 5. Asynchronous task running is easier when you combine generators and promises, because promises provide a common interface that asynchronous operations can return.
 6. Most new web APIs are being built on top of promises, and you can expect many more to follow suit in the future.
 
 
 ## 2. PROXIES AND THE REFLECTION API
Proxies were introduced in ECMAScript 6 and allows developers to create built-in objects.

**What are Proxies?** 
- Proxies are wrappers that can intercept and alter low-level JavaScript engine operations.

**What can you do with a Proxy?**
- Proxies allow you to intercept low-level object operations on the target that are otherwise internal to the JavaScript Engine. 
- Calling <ins>**new Proxy()**</ins> creates a proxy to use in place of another object (called the target). The proxy virtualizes the target so the proxy and the target appear to be functionally the same. 
- These low-level operations are intercepted using a Trap, which is a function that responds to a specific operation.
- For each Proxy Trap there is a Reflect method attached. Each trap overrides some built-in behavior of JavaScript objects, allowing you to intercept and modify the behavior. If you still need to use the built-in behavior, you can use the corresponding reflection API method.
- 

##### Reflection API 
- Is represented by the Reflect object and it is a collection of methods that provide the default behavior for the same low-level operations that proxies can override. 

| Proxy trap    | Overrides the behavior |
| :-------------|:-----------------------|
| get           | Reading a prop value   | 
| set           | Writing a prop value   | 
| has           | The in operator        | 
| deleteProperty| The delete property    | 
| getPrototypeOf| Object.getPrototypeOf()| 
| setPrototypeOf| Object.setPrototypeOf()| 
| isExtensible  | Object.isExtensible()  | 
| preventExtensions| Object.preventExtensions| 
| getOwnPropertyDescriptor| Object.getOwnPropertyDescriptor()| 
| defineProperty| Object.defineProperty()| 
| ownKeys       | Object.keys(), Object.getOwnPropertyNames(), and Object.getOwnPropertySym   | 


##### Note
  - The nonstandard behavior of numeric properties and the length property is why arrays are considered exotic objects in ECMAScript 6.
  - The original ECMAScript 6 specification had an additional trap called **enumerate** that was designed to alter how for-in and Object.keys() enumerated properties on an object. However, the enumerate trap was removed in ECMAScript 7 (also called ECMAScript 2016) because difficulties were discovered during implementation. The enumerate trap no longer exists in any JavaScript environment.
 
 #### Creating a Simple Proxy
 When you use the Proxy constructor to make a proxy, you’ll pass it two arguments: the **target** and a **handler**. 
 **What is a handler?** - A handler is an object that defines one or more traps. 
 To create a simple forwarding proxy, you can use a handler with no traps, like this:
 
```javascript 
let target = {};

let proxy = new Proxy(target, {});
proxy.name = "proxy";

console.log(proxy.name);        // "proxy"
console.log(target.name);       // "proxy"

//  Proxy forwards all operations directly to target. 
//  When "proxy" is assigned to the proxy.name property, name is created on target. 
//  The proxy is not storing this property; it’s simply forwarding the operation to target. 
//  Similarly, the values of proxy.name and target.name are the same because they are both references to target.name. 
//  That also means setting target.name to a new value causes proxy.name to reflect the same change.
target.name = "target";
console.log(proxy.name);        // "target"
console.log(target.name);       // "target"
```
**Of course, proxies without traps aren’t very interesting, so what happens when you define a trap?** 
Suppose you want to create an object whose property values must be numbers. That means every new property added to the object must be validated, and an error must be thrown if the value isn’t a number. To accomplish this task, you could define a set trap that overrides the default behavior of setting a value. The set trap receives four arguments:

```javascript 
**trapTarget** -  The object that will receive the property (the proxy’s target)
**key** - The property key (string or symbol) to write to
**value** - The value being written to the property
**receiver** - The object on which the operation took place (usually the proxy)

```

**Reflect.set()** -  is the set trap’s corresponding reflection method, and it’s the default behavior for this operation. The Reflect.set() method accepts the same four arguments as the set proxy trap, making the method easy to use inside the trap. The trap should return true if the property was set or false if not. (The Reflect.set() method returns the correct value based on whether the operation succeeded.) Here’s an example:

```javascript 

let target = {
    name: "target"
};

let proxy = new Proxy(target, {
    set(trapTarget, key, value, receiver) {

        // ignore existing properties so as not to affect them
        if (!trapTarget.hasOwnProperty(key)) {
            if (isNaN(value)) {
                throw new TypeError("Property must be a number.");
            }
        }

        // add the property
        return Reflect.set(trapTarget, key, value, receiver);
    }
    });

// adding a new property
proxy.count = 1;
console.log(proxy.count);       // 1
console.log(target.count);      // 1

// you can assign to name because it exists on target already
proxy.name = "proxy";
console.log(proxy.name);        // "proxy"
console.log(target.name);       // "proxy"

// throws an error
proxy.anotherName = "proxy";
    
```

**Code explained:** 

The _trapTarget_ value is equal to **target**, _key_ is **count**, _value_ is **1**, and _receiver_ (not used in this example) is **proxy**. There is no existing property named count in target, so the _proxy validates_ value by passing it to __isNaN()__. If the result is NaN, the property value is not numeric and an error is thrown. Because this code sets count to 1, the proxy calls Reflect.set() with the same four arguments that were passed to the trap to add the new property.
    
**Reflect.get()** -  Object Shape Validation Using the get Trap. An object shape is the collection of properties and methods available on the object. JavaScript engines use object shapes to optimize code, often creating classes to represent the objects.

Here’s an example:
```javascript 
let target = {};
//since it was not defined return undefined
console.log(target.name);    // undefined

```

In most other languages, attempting to read target.name throws an error because the property doesn’t exist. But JavaScript just uses undefined for the value of the target.name property. 

Because property validation only has to happen when a property is read, you would use the get trap. The get trap is called when a property is read, even if that property doesn’t exist on the object, and it takes three arguments:


```javascript 
**trapTarget** -  The object from which the property is read (the proxy’s target)
**key** - The property key (a string or symbol) to read.
**value** - The value being written to the property
**receiver** - The object on which the operation took place (usually the proxy)

```
These arguments mirror the set trap’s arguments but with one noticeable difference. There’s no value argument because get traps don’t write values. The __Reflect.get()__ method accepts the same **three arguments** as the _get trap()_ and returns the property’s default value. You can use the get trap and Reflect.get() to throw an error when a property doesn’t exist on the target, as follows:

```javascript 
let proxy = new Proxy({}, {
    get(trapTarget, key, receiver) {
        if (!(key in receiver)) {
            throw new TypeError("Property " + key + " doesn't exist.");
        }

        return Reflect.get(trapTarget, key, receiver);
    }
});

// adding a property still works
proxy.name = "proxy";
console.log(proxy.name);            // "proxy"

// nonexistent properties throw an error
console.log(proxy.nme);             // throws an error

```
**The _in_ Operator - or how to detect if an object has a certain property**

The _**in**_ operator determines whether a property exists on a given object and returns true if an own property or a prototype property matches the name or symbol. For example:

```javascript 
let target = {
    value: 42;
}

console.log("value" in target);     // true
console.log("toString" in target);  // true
```
Both _value_ and _toString_ exist on object, so in both cases the in operator returns true. The value property is an own property, whereas toString is a prototype property (inherited from Object). Proxies allow you to intercept this operation and return a different value for in with the has trap. 
The has trap is called whenever the _**in Operator**_ is used. When called, two arguments are passed to the has trap:

```
trapTarget -  The object the property is read from (the proxy’s target)
key - The property key (string or symbol) to check

```
  
**Reflect.has() Method**
Reflect.has() method accepts these same arguments and returns the default response for the _**in Operator**_. Using the has trap and Reflect.has() allows you to alter the behavior of in for some properties while reverting to the default behavior for others. For instance, you could hide the value property from the previous example like this:

```javascript
let target = {
    name: "target",
    value: 42
};

let proxy = new Proxy(target, {
    has(trapTarget, key) {

        if (key === "value") {
            return false;
        } else {
            return Reflect.has(trapTarget, key);
        }
    }
});
console.log("value" in proxy);      // false
console.log("name" in proxy);       // true
console.log("toString" in proxy);   // true
```

### deleteProperty Trap
The delete operator removes a property from an object and returns true when it’s successful and false when it’s unsuccessful. In strict mode, delete throws an error when you attempt to delete a nonconfigurable property; in non-strict mode, delete simply returns false. Here’s an example:

```javascript
let target = {
    name: "target",
    value: 42
};
Object.defineProperty(target, "name", { configurable: false });

console.log("value" in target);     // true

let result1 = delete target.value;
console.log(result1);               // true

console.log("value" in target);     // false

// note: the following line throws an error in strict mode
let result2 = delete target.name;
console.log(result2);               // false

console.log("name" in target);      // true

```
## 3. STRINGS AND REGULAR EXPRESSIONS
In ECMAScript 6 strings and regular expressions are improved, by adding new capabilities and long-missing functionality. 

In ECMAScript 5, all string operations work on 16-bit code units, meaning that you can get unexpected results from UTF-16 encoded strings containing surrogate pairs, as in this example:

```javascript
let text = "𠮷";

console.log(text.length);           // 2
console.log(/^.$/.test(text));      // false
console.log(text.charAt(0));        // ""
console.log(text.charAt(1));        // ""
console.log(text.charCodeAt(0));    // 55362
console.log(text.charCodeAt(1));    // 57271

```
The single Unicode character "𠮷" is represented using surrogate pairs, and as such, the JavaScript string operations in this example treat the string as having two 16-bit characters. That means:
• The length of text is 2 when it should be 1.
• A regular expression trying to match a single character fails because it thinks there are two characters.
• The charAt() method is unable to return a valid character string because neither set of 16 bits corresponds to a printable character.
• The charCodeAt() method also can’t identify the character properly. It returns the appropriate 16-bit number for each code unit, but that is the closest you could get to the real value of text in ECMAScript 5.

ECMAScript 6 enforces UTF-16 string encoding to address problems like those mentioned above. Standardizing string operations based on this character encoding means that JavaScript can support functionality designed to work specifically with surrogate pairs. 

ECMASCRIPT6 Methods added to fully support UTF-16 are:
  - **codePointAt()** -  retrieves the Unicode code point that maps to a given position in a string. This method accepts the code unit position rather than the character position and returns an integer value. 
  
 ```javascript
 let text = "𠮷a";

console.log(text.charCodeAt(0));    // 55362
console.log(text.charCodeAt(1));    // 57271
console.log(text.charCodeAt(2));    // 97

console.log(text.codePointAt(0));   // 134071
console.log(text.codePointAt(1));   // 57271
console.log(text.codePointAt(2));   // 97

``` 
Calling the codePointAt() method on a character is the easiest way to determine whether that character is represented by one or two code points.

```javascript 
  function is32Bit(c) {
    return c.codePointAt(0) > 0xFFFF;
}

console.log(is32Bit("𠮷"));          // true
console.log(is32Bit("a"));          // false
``` 
  - **fromCodePoint()** - retrieve the code point for a character in a string, whereas String.fromCodePoint() produces a single-character string from a given code point. 
  
 ```javascript
console.log(String.fromCodePoint(134071));  // "𠮷"
``` 

 - **normalize()** - optionally accepts a single string parameter that indicates that one of the following Unicode normalization forms should be applied:
 • Normalization Form Canonical Composition ("NFC"), the default
 • Normalization Form Canonical Decomposition ("NFD")
 • Normalization Form Compatibility Composition ("NFKC")
 • Normalization Form Compatibility Decomposition ("NFKD")

If you ever work on an internationalized application, you’ll definitely find the normalize() method helpful.
Keep in mind that when you’re comparing strings, both strings must be normalized to the same form. For example:


```javascript
let normalized = values.map(function(text) {
    return text.normalize();
});

normalized.sort(function(first, second) {
    if (first < second) {
    return -1;
    } else if (first === second) {
        return 0;
    } else {
        return 1;
    }
});
      
``` 
New methods aren’t the only improvements that ECMAScript 6 provides for working with Unicode strings. ECMAScript 6 also introduces the regular expression u flag and other changes to strings and regular expressions.

###### The Regular Expression _u Flag_

Regular expressions assume 16-bit code units, where each represents a single character. To address this problem, ECMAScript 6 defines a u flag (which stands for Unicode) for use in regular expressions.

_**The u Flag in Action**_
When a regular expression has the u flag set, it switches modes to work on characters, not code units. That means the regular expression should no longer treat surrogate pairs as separate characters in strings and should behave as expected. For example, consider this code:

```javascript
let text = "𠮷";

console.log(text.length);           // 2
console.log(/^.$/.test(text));      // false
console.log(/^.$/u.test(text));     // true
```
The regular expression /^.$/ matches any input string with a single character. When it’s used without the u flag, this regular expression matches on code units, so the Japanese character (which is represented by two code units) doesn’t match the regular expression. When it’s used with the u flag, the regular expression compares characters instead of code units, so the Japanese character matches.

###### Methods for Identifying Substrings
Developers have used the indexOf() method to identify strings inside other strings since JavaScript was first introduced.
ECMAScript 6 includes the following 3 new methods, which are designed to identify strings inside other strings: 
  - **includes()** - returns true if the given text is found anywhere within the string. It returns false if not.
  - **startsWith()** - returns true if the given text is found at the beginning of the string. It returns false if not.
  - **endsWith()** - returns true if the given text is found at the end of the string. It returns false if not.
Each method accepts two arguments: the text to search for and an optional index from which to start the search. 
When the second argument is provided, includes() and startsWith() start the match from that index, and endsWith() starts the match from the length of the string minus the second argument; 
When the second argument is omitted, includes() and startsWith() search from the beginning of the string, and endsWith() starts from the end.
Here are some examples showing these three methods in action:
```javascript
let msg = "Hello world!";

console.log(msg.startsWith("Hello"));      // true
console.log(msg.endsWith("!"));            // true
console.log(msg.includes("o"));            // true

console.log(msg.startsWith("o"));          // false
console.log(msg.endsWith("world!"));       // true
console.log(msg.includes("x"));            // false

console.log(msg.startsWith("o", 4));       // true
console.log(msg.endsWith("o", 8));         // true
console.log(msg.includes("o", 8));         // false
```

> NOTE: 
> _The startsWith(), endsWith(), and includes() methods will throw an error if you pass a regular expression instead of a string. In 
> contrast, indexOf() and lastIndexOf() convert a regular expression argument into a string and then search for that string._

###### The repeat() Method
ECMAScript 6 also adds a repeat() method to strings, which accepts the number of times to repeat the string as an argument. It returns a new string containing the original string repeated the specified number of times. For example:

```javascript
console.log("x".repeat(3));        // "xxx"
console.log("hello".repeat(2));    // "hellohello"
console.log("abc".repeat(4));      // "abcabcabcabc"
```

###### Template Literals
ECMAScript 6’s answer to the following features that JavaScript lacked in ECMAScript 5 and in earlier versions:
 - Multiline strings A formal concept of multiline strings;
 - Basic string formatting The ability to substitute parts of the string for values contained in variables;
 - HTML escaping The ability to transform a string so it is safe to insert into HTML;

At their simplest, template literals act like regular strings delimited by backticks instead of double or single quotes. For example, consider the following:

```javascript
let message = `\`Hello\` world!`;

console.log(message);             // "`Hello` world!"
console.log(typeof message);      // "string"
console.log(message.length);      // 14
```

Also ECMAScript 6’s template literals make **multiline strings** easy because there’s no special syntax. Just include a newline where you want, and it appears in the result, like so:


```javascript
//In this code, all whitespace before the second line of the template literal is considered part of the string.
let message = `Multiline
               string`;

console.log(message);           // "Multiline
                                //                  string"
console.log(message.length);    // 31
```
The real difference between template literals and normal versions of JavaScript strings are **Substitutions**. 
**Substitutions** allow you to embed any valid JavaScript expression inside a template literal and output the result as part of the string. They are delimited by an opening **${ and a closing }** that can have any JavaScript expression inside. The simplest substitutions let you embed local variables directly into a resulting string, like this:

```javascript
let name = "Nicholas",
    message = `Hello, ${name}.`;
    
console.log(message);       // "Hello, Nicholas."
```
Because all substitutions are JavaScript expressions, you can substitute more than just simple variable names. You can easily embed calculations, function calls, and more. For example:

```javascript
let count = 10,
    price = 0.25,
    message = `${count} items cost $${(count * price).toFixed(2)}.`;

console.log(message);       // "10 items cost $2.50."
```

Also you can place a template literal inside another template literal, as in this example:

```javascript
let name = "John Doe",
    message = `Hello, ${
        `my name is ${ name }`
    }.`;

console.log(message);        // "Hello, my name is John Doe.""
```

> NOTE:
> A template literal can access any variable accessible in the scope in which it is defined. Attempting to use an undeclared variable in > a template literal throws an error in strict and non-strict modes.

###### TAGGED TEMPLATES

The real power of template literals is represented by <ins>**TAGGED TEMPLATES**</ins>.
Tagged templates - allow you to parse template literals with a function. The first argument of a tag function contains an array of string values. The remaining arguments are related to the expressions. In the end, your function can return your manipulated string (or it can return something completely different as described in the next example). The name of the function used for the tag can be whatever you want.

```javascript
var person = 'Mike';
var age = 28;

function myTag(strings, personExp, ageExp) {
  var str0 = strings[0]; // "That "
  var str1 = strings[1]; // " is a "

  // There is technically a string after
  // the final expression (in our example),
  // but it is empty (""), so disregard.
  // var str2 = strings[2];

  var ageStr;
  if (ageExp > 99){
    ageStr = 'centenarian';
  } else {
    ageStr = 'youngster';
  }

  // We can even return a string built using a template literal
  return `${str0}${personExp}${str1}${ageStr}`;
}

var output = myTag`That ${ person } is a ${ age }`;

console.log(output);
// That Mike is a youngster
```

###### Raw strings
 - The special raw property, available on the first argument to the tag function, allows you to access the raw strings as they were entered, without processing escape sequences: 
 
 ```javascript
function tag(strings) {
  console.log(strings.raw[0]);
}

tag`string text line 1 \n string text line 2`;
// logs "string text line 1 \n string text line 2" ,
// including the two characters '\' and 'n'
```

## 4. BLOCK BINDINGS










