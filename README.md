# Advanced Key-Concepts-in-Javascript by Cosmina Palade
A comprehensive and easy to understand guide for Javascript advanced concepts with examples and details. 
Information is structured and gathered from different resources, mentioned in the Reference part of the book. 


## Promises 
### What is a PROMISE? 
A promise is an Object that keeps track whether a certain event has happened already or not and determines what happens after the event ends. A Promise implements the concept of a future value that we’re expecting.
“It’s saying - hey get me some data from the server in the background and it promises that it will get that data”;
In a PROMISE we are dealing with time sensitive code thus a PROMISE can have different states: 
  - **Pending** - before the event has happening, the Promise is pending, 
  - **Settled, Resolved** -  after the event has happen;
  - **Fulfilled** - when the promise what settled, resolved successful;
  - **Rejected** - if there was an error;

In more practical terms we can Produce and Consume Promises; so when we produce a Promise we create a new Promise and send the result using that Promise, than when we consume it, we can use callback functions for fulfillment or rejection of that promises.




