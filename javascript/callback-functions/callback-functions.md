# Callback Functions in JavaScript

Callback functions are useful in various scenarios, particularly when dealing with asynchronous operations. They are often used in situations where you need to wait for a specific operation to complete before executing another piece of code. Some common use cases for callback functions in JavaScript include:

## 1. Event Handling

Callback functions are used to define event listeners, which are executed when a specific event occurs, such as a button click or a form submission.

```javascript
button.addEventListener('click', () => {
  console.log('Button clicked!');
});
```

## 2. Asynchronous Data Fetching

Callbacks are used when making asynchronous requests, such as fetching data from a server using XMLHttpRequest or the Fetch API.

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => {
    console.log('Data fetched:', data);
  })
  .catch(error => {
    console.error('Error fetching data:', error);
  });
```

## 3. Timers

JavaScript provides setTimeout and setInterval functions, which execute a callback function after a specified delay or at regular intervals.

```javascript
setTimeout(() => {
  console.log('This message is displayed after 3 seconds');
}, 3000);
```

## 4. Array Methods

JavaScript arrays have several methods like map, filter, and forEach that accept callback functions to perform specific operations on each element of the array.

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubledNumbers = numbers.map(number => number * 2);
console.log(doubledNumbers);
```

Refer this small example - https://stackblitz.com/edit/js-iddccl?file=index.js
