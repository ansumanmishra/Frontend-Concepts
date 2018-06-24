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