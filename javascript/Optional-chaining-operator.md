## Optional chaining operator

### For simple object properties
```javascript
const user = {
  name: 'Ansuman',
  address: {
    city: 'Bern'
  }
};
console.log(user.address.city); // Bern
```

However if the object does not have address property

```javascript
const user = {
  name: 'Ansuman'
};
console.log(user.address.city); // Uncaught TypeError: Cannot read property 'city' of undefined
```
then calling name property throws error

This can be avoided by tradional way of writing if statement (Or using ternary operator) to check if address property exists before calling city.

```javascript
if (user.address && user.address.city) {}
```

This can be simplified using the optional chaning operator:

```javascript
console.log(user?.address?.city); // Undefined
```
This will silently fail without throwing any error 🙂

It's supported on most modern browsers - [Browser Support]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining

### Functions
Optional chaining operator can be used for funtions as well
```javascript
const user = {
  name: 'Ansuman',
  address: {
    city: 'Bern',
    country: 'Switzerland',
    getAddress: () => {
			console.log(`Getting address`);
    }
  }

};

console.log(user?.address.getAddress?.()); // Getting address
```
Here if the getAddress function does not exist then it fails silently and does not throw error
```javascript
const user = {
  name: 'Ansuman',
  address: {
    city: 'Bern',
    country: 'Switzerland'
  }

};
console.log(user?.address.getAddress?.()); // undefined
```
### Arrays
It even works well with arrays
```javascript
const user = {
  name: 'Ansuman',
  address: [{
    city: 'Bern',
  }, {
    city: 'Bhubaneswar'
  }]

};
console.log(user?.address?.[0].city); // Bern
```
