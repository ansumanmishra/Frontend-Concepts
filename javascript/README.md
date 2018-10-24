# Javascript Interview Questions & Answers

## Scope
Local scope - a variable can be accessed in a particular place
Global scope - a variable can be accessed from anywhere

## Hoisting
It is JS's default behavior of moving the variables and function declarations to the top.

```javascript
console.log(x) // Error

console.log(x) // Undefined
var x
```

## How do you check if a variable is an object
You can use typeof to determine if variable is an object, however bear in mind that null is actually an object! However null object is 'falsy' thus the following will work:

```javascript
if(bar && typeof bar === "object") {
	console.log('bar is object and is not null');
}
```

## Closures
A closure is a function defined inside another function (called parent function) and has access to the variable which is declared and defined in parent function scope.

```javascript
function sayHelloCreator(name) {
	var greeting = 'Hello'

	function sayHelloFunction() {
		console.log(greeting + ' ' + name);
	}

	return sayHelloFunction;
}

var sayHelloToJohn = sayHelloCreator('John');
sayHelloToJohn() // Hello John

```

## Functional Programming

Do everything with functions. There are imperative style (do this, do that step by step) & object oriented programming where everything is done on top of objects.
Its a coding pattern or mindset to do things using functions. Expressing everything in functions like taking inputs and giving outputs.

Always use pure functions.
Pure functions: Dont take anything from global to compute anything. Use only arguments being passed.

Not pure:
```javascript
var name = "Ansuman";

function greet() {
	return name
}
```

Pure function:
```javascript
function greet(name) {
	return name
}

greet('Ansuman')
```

It involves using pure functions that avoid shared state, mutating data and side-effects.

Imperative way:
```javascript
var name = "Ansuman";
var greeting = "Hi, I'm";
console.log(greeting + name);
```

Functional way:
```javascript
function greet(name) {
	return "Hi I'm" + name
}

greet("Ansuman");
```

**Use Higher order functions** - function within function which return something

**Dont mutate data**

```javascript
var obj = {
	x: 4
}

var newObj = function() {
	obj.x = obj.x + 1
}
```

**Functional way**
Create a copy of the object first.

```javascript
var obj = {
	x: 4
}

var addFunc = function(obj) {
	var newObj = {
		x: obj.x
	}

	newObj.x = newObj.x + 1

	return newObj;
}
```

## Object.assign
Copy properties and methods from another object.

```javascript
let x = {
	name: "Ansuman"
}

let y = {
	age: 30
}

Object.assign(y, x);

console.log(y) // Prints {age: 30, name: "Ansuman"}
```

Copying all objects

```javascript
let copiedObj = Object.assign({}, x);
```

## Object.create
Create an object from an existing object
```javascript
let x = {
	name: "Ansuman"
}


var y = Object.create(x, {});

console.log(y.name) // Prints Ansuman
```

## Synchronous & Asynchronous code

**Synchronous** code is blocking
```javascript
console.log(1)
console.log(2)
// prints 1
// Prints 2
```

**Asynchronous** code is nonblocking
```javascript
console.log(1)
setTimeout(function() {
	console.log(2)
}, 1000);
// prints 2
// Prints 1	
```

## How to check if something as an array?
If we do typeof [] // object

correct way to check is:
```javascript
[].constructor; // Array
[].constructor === Array; // true
```

## Higher order function
**Higher order function** is a function which takes a function as an argument or returns a function as a result. Its a key concept for functional programming. Closure is a kind of higher order function

## Difference between array and array like object
Array like object does not have standard array methods but can be converted to an array using Array.from()

Array like object example:

```javascript
function() {
	console.log(arguments) // This is a array like object, does not array properties like push, slice. Only length works
}
```

convery array like object to array
```javascript
Array.prototype.slice.call(arguments)
```

## Type Coercion
JS sometimes allows something of a particular type to be coerced into another type.

```javascript
console.log('hi' + 8) // 'hi 8'
console.log('8' == 8) // true
```

## Two way data binding & one-way data flow
**Two way data binding:** UI and model change together

**One-way data flow:** Single source of truth, changes to that single source of truth can only flow in one direction

## Prototypal/Differential inheritance
Conceptually this is very simple: A new object can inherit properties of an old object.

Example 1
```javascript
(function() {
	var genericObject = {
		bar : "Hello World",
		get_bar : function() {
			return this.bar;
		}
	};
	var customObject = Object.create(genericObject);
	customObject.bar = "Aloha folks!";
	console.log(customObject.get_bar()); //outputs: "Aloha folks"
	delete customObject.bar;
	console.log(customObject.get_bar()); //fallbacks to the prototype's value, outputs: "Hello World"
})();
```

Example 2
When we create an object from an existing object, the newly created object will inherit the properties and methods from the parent object. This is called as prototypal inheritance
```javascript
let car = function(model) {
	this.model = model;
}

car.prototype.getModel = function() {
	return this.model
}

var bmw = new car('bmw');
console.log(bmw.getModel()); // Prints bmw

```
## Prototype Chain
**Add it here**

## How to empty an array in JavaScript?
There are 3 ways to do so:
```javascript
arrayList = [];
arrayList.length = 0;
arrayList.splice(0, arrayList.length);
```

## Difference between call, apply & bind
The difference is that apply lets you invoke the function with arguments as an array; call requires the parameters be listed explicitly. A useful mnemonic is "A for array and C for comma."

Call example:
```javascript
var obj = {
	num1: 5
};

var func = function(num2, num3) {
	return this.num1 + num2 + num3
}

console.log(func.call(obj, 3, 4)) // Prints 12
```

Apply example:
```javascript
var numArr = [3, 4]
console.log(func.apply(obj, numArr)) // Prints 12
```

Bind example:
```javascript
var bound = func.bind(obj) // It returns the function
console.log(bound(4, 5)) // Prints 14
```

## Event capture & Event bubbling
Event bubbling and capturing are two ways of event propagation in the HTML DOM API, when an event occurs in an element inside another element, and both elements have registered a handle for that event. The event propagation mode determines in which order the elements receive the event.

With bubbling, the event is first captured and handled by the innermost element and then propagated to outer elements.
child->parent

With capturing, the event is first captured by the outermost element and propagated to the inner elements.
parent->child

## Why "use strict"?
Makes debugging easier - Code errors that would otherwise have been ignored or would have failed silently will now generate errors or throw exceptions, alerting you sooner to problems in your code and directing you more quickly to their source

Prevents accidental globals
Eliminates this coercion - Without strict mode, a reference to a this value of null or undefined is automatically coerced to the global. This can cause many headfakes and pull-out-your-hair kind of bugs. In strict mode, referencing a a this value of null or undefined throws an error.
Disallows duplicate parameter values

## How to check if an object is array?
```javascript
Array.isArray
v instanceof Array
$.isArray() // jquery method
obj.constructor === Array
```

## What is an Associative array
```javascript
var myObj = {a: 200, b: 300};

// Creating an associative array using the bracket notation:
var myArray = []; // creating a new array object
myArray['a'] = 200; // setting the attribute a to 200
myArray['b'] = 300;  // setting the attribute b 300
```

Check the length:
```javascript
Object.keys(counterArray).length; // Output 3
```

## What are the way by which we can create object in JavaScript ?
**Function based**
```javascript
function Employee(fName, lName, age, salary){
	this.firstName = fName;
	this.lastName = lName;
	this.age = age;
	this.salary = salary;
}

// Creating multiple object which have similar property but diff value assigned to object property.
var employee1 = new Employee('John', 'Moto', 24, '5000$');
var employee1 = new Employee('Ryan', 'Jor', 26, '3000$');
```

**Object Literal**
```javascript
var employee = {
	name : 'Ansuman',
	salary : 245678,
	getName : function(){
		return this.name;
	}
}
```

**Using JavaScript new keyword**
```javascript
var employee = new Object(); // Created employee object using new keywords and Object()
employee.name = 'Ansuman';
```

## Explain this keyword in Javascript
**this** keyword is bind to the function where it is executed and not where it is defined. 
Execute a function in global env - this refers to global scope.
If there is an object a  and method b inside it. here in this case "this" will bind to a

## How to check whether a key exist in a JavaScript object or not?
console.log('name' in person); // checking own property print true 
console.log('salary' in person); // checking undefined property print false

## Object-Based inheritance
```javascript
var employee = {
  name: 'Nishant',
  displayName: function () {
    console.log(this.name);
  }
};

var emp1 = Object.create(employee);
console.log(emp1.displayName());  // output "Nishant"
```

## Merge two JavaScript Object dynamically
```javascript
Object.assign(person,location); // ES6
```

## Method Override
```javascript
obj.prototype.methodtobeoverridden = function() {
	
}
```

## Namespace
Namespacing is a technique employed to avoid collisions with other objects or variables in the global namespace

```javascript
var yourNamespace = {

    foo: function() {
    },

    bar: function() {
    }
};

yourNamespace.foo();
```

## Javascript best practices
 - Avoid polluting the global scope
 - use strict
 - Strict equal
 - Avoiding multiline var statements
 - Using object lateral syntax
 - Putting semicolons everyline end


## ES6 Features

### Modules
### Classes
#### Class inheritance
### Arrow Functions

### Default parameters
```javascript
function add(a=0, b=0);

console.log(add()) // prints 0
console.log(add(2, 3)) // prints 5
```

### Template Literals
```javascript
var name = "Ansuman";
var myname = `My name is ${name} and I stay in India`
```

### Destructuring
Example 1
```javascript
const person = {
  first: 'Wes',
  last: 'Bos',
  country: 'Canada',
  city: 'Hamilton',
  twitter: '@wesbos'
};

// To get first and last name instead of doing this
const first = person.first;
const last = person.last;

// We can do the following using Destructuring
const { first, last } = person;
```
Example 2
```javascript
var user = {
	name: 'Ansuman',
	age: '31'
};

var {name, age} = user;

console.log(name) // Ansuman
```

### Enhanced Object Literals
### Multi-line Strings
### Let and Const
### Promises
Promise has 3 states:
 - pending
 - resolved
 - rejected

```javascript
var promise = new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…

  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
});

promise.then(function(result) {
  console.log(result); // "Stuff worked!"
}, function(err) {
  console.log(err); // Error: "It broke"
});
```

### Spread operators
```javascript
var x = function(...n) {
  console.log(n) // prints the array [1, 2, 3]
}
x(1, 2, 3);

var a = [1, 2, 3];
var b = [...a, 4, 5] // prints [1, 2, 3, 4, 5]

var a = [1, 2, 3];
var b = [5, 6, 7];

var c = a.push(...b); // prints [1, 2, 3, 5, 6, 7]
```

### Enhanced Regular Expression
### Enhanced Object Properties
Example
```javascript
//es5: 
obj = { x: x, y: y };
//es6: 
obj = { x, y }
```

### Iterators
An object is an iterator when it knows how to access items from a collection one at a time, while keeping track of its current position within that sequence

Symbol.iterator
next()

```javascript
function makeIterator(array) {
    var nextIndex = 0;
    
    return {
       next: function() {
           return nextIndex < array.length ?
               {value: array[nextIndex++], done: false} :
               {done: true};
       }
    };
}
var it = makeIterator(['yo', 'ya']);
console.log(it.next().value); // 'yo'
console.log(it.next().value); // 'ya'
console.log(it.next().done);  // true
```

### Generators
While custom iterators are a useful tool, their creation requires careful programming due to the need to explicitly maintain their internal state. Generators provide a powerful alternative: they allow you to define an iterative algorithm by writing a single function which can maintain its own state.

```javascript
function* idMaker() {
  var index = 0;
  while(true)
    yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
```

## ES8

### Object.values(), Object.entries
### String padding
```javascript
string.padStart(10, ' ')
```

### Trailing commas
```javascript
console.log('abc',)
```

### Async functions

## Progessive Webapp

PWA are essentially fast, performance-focused web applications that are streamlined for mobile. They also can be saved to your smartphone’s home screen and, from there, look and feel like a native app (including features like offline access and push notifications)

 - Step 1 - Lighthouse - Lighthouse is a free tool from Google that evaluates your app based on their PWA checklist. Its a chrome extension
 - Step 2 - Set Up A Service Worker - For caching & offline access
 - Step 3 - Add Progressive Enhancement - Progressive enhancement basically means your site will work without any JavaScript loading
 - Step 4 - Add To Home Screen Capability
 - Step 5 - Deploy Via Firebase

## Reduce
Calculate the sum/average of an array elements

```javascript
var a = [1, 2, 3, 4, 5];

var sum = a.reduce( (total, amount) => total + amount )
console.log(sum) //15
```

## Map
Do something to the individual elements of an array
```javascript
var a = [2, 3, 5];
var b = a.map( (i) => i/2 )
console.log(b) //[1, 1.5, 2.5]
```

## Enhanced object literals
```javascript
var x = 10;
var y = 20;

//ES5
var obj = {
	x: x,
	y: y,
	add: function() {
		return x + y
	}
}

//ES6
var obj = {
	x, // Enhanced way of assigning values to properties
	y,
	add() { // Enhanced way of defining methods
		return x + y
	}
}

console.log(obj.add()) // 30
```

## Mutable vs Immutable

Mutable object - The object is subject to be changed/altered.
Immutable object - The object cannot be changed once created.

Here is an example of mutating your original object in JavaScript,
```javascript
  function mutation(originalArray) {
    // directly mutating/modifying the original array
    originalArray[0] = "new value";
    return originalArray;
  }

  var array = ["some value", "another value"];
  alert("Return from mutation " + mutation(array));
  alert("Array: " + array + " (original array has been altered).");
  ```
  
  In this example, the original array got changed in the function (the object has been mutated).

Here is an example of immutable-object-style coding in JavaScript,
```javascript
  function immutable(originalArray) {
    // Instead of mutating/modifying the original array,
    // we first make a copy of the original array
    // In this way, the original array stay unchanged.
    var newArray = [...originalArray];
    newArray[0] = "new value";
    return newArray;
  }

  var array = ["some value", "another value"];
  alert("Return from immutable " + immutable(array));
  alert("Array: " + array + " (original array stay unchanged).");
  ```
  
In this example, the original array stay unchanged even though it was used in the function (a new copied/created array has been return with the new changes).

Both approach might be perfectly fine for small simple program. However, as your application scales/grows, you might prefer one way or the other.

## More on Mutable vs Immutable
A mutable object is an object whose state can be modified after it is created. An immutable object is an object whose state cannot be modified after it is created

Mutable - Strings and Numbers
Immutable - Objects and Arrays

Examples:

``` javascript
let a = {
    foo: 'bar'
};

let b = a;

a.foo = 'test';

console.log(b.foo); // test
console.log(a === b) // true
```

``` javascript
let a = 'test';
let b = a;
a = a.substring(2);

console.log(a) //st
console.log(b) //test
console.log(a === b) //false
```

``` javascript
let a = ['foo', 'bar'];
let b = a;

a.push('baz')

console.log(b); // ['foo', 'bar', 'baz']
console.log(a === b) // true
```

``` javascript
let a = 1;
let b = a;
a++;

console.log(a) //2
console.log(b) //1
console.log(a === b) //false
```

What we see is that for mutable values, updating state applies across all references to that variable. So changing a value in one place, changes it for all references to that object. For the immutable data types, we have no way of changing the internal state of the data, so the reference always gets reassigned to a new object.

Creating immutable objects
### Object.freeze()

The Object.freeze() method freezes an object. A frozen object can no longer be changed; freezing an object prevents new properties from being added to it, existing properties from being removed, prevents changing the enumerability, configurability, or writability of existing properties, and prevents the values of existing properties from being changed. In addition, freezing an object also prevents its prototype from being changed. freeze() returns the same object that was passed in.

``` javascript
const object1 = {
  property1: 42
};

const object2 = Object.freeze(object1);

object2.property1 = 33;
// Throws an error in strict mode

console.log(object2.property1);
// expected output: 42
```
### To enforce object immutability while update the object make sure to use Object.assign() rather than object.prop='something'

``` javascript
let a = {
	name: 'Ansuman'
};

var b = Object.assign({},a);

b.name = 'mishra';

console.log(a); // Ansuman
console.log(b); // mishra
```

### we can use spread(...) operator.

``` javascript
var person={name:'Ansuman',age:26}
var newPerson={...person,name:'Mishra'}

console.log(newPerson ===person) //false
console.log(person)  //{name:'Ansuman',age:26}
console.log(newPerson)  //{name:'Mishra',age:26}
```
