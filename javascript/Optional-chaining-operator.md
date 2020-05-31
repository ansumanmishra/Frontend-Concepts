## Optional chaining operator
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
This will silently fail without throwing any error
