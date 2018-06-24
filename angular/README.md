# Angular Interview Questions & Answers

## Interacting with components

### Parent to Child

parent-component.ts
```javascript
public title:String = 'Page title';
```
parent-component.html
```html
<app-users [title]="title"></app-users>
```

child-component.ts
```javascript
//Step 1
import { Component, OnInit, Input } from '@angular/core';

//Step 2
@Input() title:String;
```

### Child to Parent

child-component.ts
```javascript
// Step 1
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

//Step 2 
@Output() msg = new EventEmitter();

//Step 3: On any action 
this.msg.emit('Some message')
```
parent-component.html
```html
<app-users (msg)="userClicked($event)"></app-users>
```

parent-component.ts
```javascript
userClicked($event) {
  console.log($event);
}
```

### Unrelated components

## Making HTTP calls
users.service.ts
 - Import the modules required for AJAX calls

```javascript
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

```
 - Add the following to constructor

```javascript
   constructor(private http: HttpClient) { }
```

 - Function for getting the users

```javascript
getUsersList(): Observable {
  return this.http
         .get('https://randomuser.me/api/?results=10').pipe(
         .catchError(err => throwError(err.message))
         )  
}
  ```

In the users.components.ts file:
 - Import the service

```javascript
import { UsersService } from '../users.service';
```
 - Add this to constructor

```javascript
constructor(private _usersService: UsersService) { }
```

- In the ngOnInit add the following:

```javascript
ngOnInit() {  
  //this.usersList = this._usersService.getUsers();
  this._usersService.getUsersList()
    .subscribe( data => this.usersList = data['results'], 
        err => this.error = err);
}
```