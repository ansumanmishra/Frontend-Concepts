# Promises

## Basic concept
Promises provide a more structured and standardized approach to handling asynchronous operations, avoiding some of the issues associated with callback functions.

```javascript
function getData(action) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      action === 'success'
        ? resolve('Sending data')
        : reject('Failure sending data');
    }, 2000);
  });
}

getData('success').then((res) => {
  console.log(res);
});

getData('error')
  .then((res) => {
    console.log(res);
  })
  .catch((err) => {
    console.log(err);
  });
```

Example - https://stackblitz.com/edit/js-s4y3vj?file=index.js

Advantages of promises over callback functions:
- Chaining: Promises can be chained using .then() and .catch(), which allows for a more readable and maintainable code structure when handling multiple dependent asynchronous operations.

- Error Handling: Promises have a standardized way of handling errors with the .catch() method, which leads to more consistent error handling.

## Promise.all()

## Promise.race()

## Promise.allSettled()

## Promise.any()