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
#### Using Subject in Service

An observable allows you to subscribe only whereas a subject allows you to both publish and subscribe. So a subject allows your services to be used as both a publisher and a subscriber.

Create the service

```javascript
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs/Subject';

@Injectable()
export class MessageServiceService {

  message: Subject<string> = new Subject<string>();

  constructor() { }

  createMessage(msg) {
    this.message.next(msg);
  }
}
```

Setting the 'message' from one component

```javascript
import { Component, OnInit } from '@angular/core';
import { MessageServiceService } from './message-service.service';

@Component({
  selector: 'app-message',
  templateUrl: './message.component.html',
  styleUrls: ['./message.component.css']
})
export class MessageComponent implements OnInit {

  constructor(private messageService: MessageServiceService) { }

  ngOnInit() {
    setTimeout( () => {
      this.messageService.createMessage('hello world')
    }, 5000);
  }

}
```

Subscribing and getting the 'message' value in other component

```javascript
import { Component, OnInit } from '@angular/core';
import { MessageServiceService } from './message/message-service.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  title = 'app';
  message: string = '';

  constructor(private messageService: MessageServiceService) {

  }

  ngOnInit() {
    this.messageService.message.subscribe( (message) => {
      this.message = message;
    });
  }
}
```

## Making HTTP calls
app.module.ts
 - Import the HttpClientModule

```javascript
import { HttpClientModule } from '@angular/common/http';
```
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

## Reactive forms
### Form field value changes

1 - First, let’s initialize our reactive form with FormBuilder
```javascript
myForm: FormGroup;
formattedMessage: string;

constructor(private formBuilder: FormBuilder) {}

ngOnInit() {
  this.myForm = this.formBuilder.group({
    name: '',
    email: '',
    message: ''
  });

  this.onChanges();
}
```

2 - Here’s the content of our onChanges method:
```javascript
onChanges(): void {
  this.myForm.valueChanges.subscribe(val => {
    this.formattedMessage =
    `Name is ${val.name} and my email is ${val.email}.
  });
}
```

If we want to listen for changes on specific form controls:
```javascript
onChanges(): void {
  this.myForm.get('name').valueChanges.subscribe(val => {
    this.formattedMessage = `My name is ${val}.`;
  });
}
```

By default, the value and validation status of a FormControl updates whenever its value changes. If an application has heavy validation requirements, updating on every text change can sometimes be too expensive.

This commit introduces a new option that improves performance by delaying form control updates until the "blur" event. To use it, set the updateOn option to blur when instantiating the FormControl.

```javascript
// example without validators
const c = new FormControl('', { updateOn: 'blur' });

// example with validators
const c= new FormControl('', {
   validators: Validators.required,
   updateOn: 'blur'
});
```

### Setting form value
#### For set value for one form element:
```javascript
this.signupForm.controls['name'].setValue('Ansuman');
```

However this won't work:

```javascript
this.signupForm.setValue({
  name: 'Ansuman'
});
```

In the above scenario, we have to set values for all elements in the form. So we need to use patchValue
```javascript
this.signupForm.patchValue({
  name: 'Ansuman'
});
```

#### To set all FormGroup values use, setValue:
```javascript
this.signupForm.setValue({
  country: 'India',
  state: 'Odisha',
  pin: '751021'
});
```

#### To set only some values, use patchValue:
```javascript
this.signupForm.patchValue({
  country: 'India'
});
```

```javascript
#### For set value when your control is FormGroup:
this.signupForm.controls['address'].patchValue({
  country: 'India',
  state: 'Odisha',
  pin: '751021'
});
```

### Populating dropdown values
In the component:

```javascript
countries: string[] = ['India', 'Switzerland', 'USA', 'UK', 'Canada'];
```

In the template:
```html
<select formControlName="country">
  <option value="">Select Country</option>
  <option *ngFor="let country of countries" [ngValue]="country">{{country}}</option>
</select>
```

Getting the dropdown change value:
Option 1
```javascript
this.signupForm.get('country').valueChanges.subscribe( (val) => {
  console.log(val)
});
```
If the field in it's own formgroup
```javascript
this.signupForm.controls['address'].get('country').valueChanges.subscribe( (val) => {
  console.log(val)
});
```
Option 2
Adding the onchange event in the dropdown in template
```html
<select formControlName="country" (change)="onSelectCountry($event.target.value)">
```
In the component
```javascript  
onSelectCountry(country) {
  console.log(country)
}
```
### Form Validations
In the component:

```javascript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
 
@Component({
    selector: 'app',
    templateUrl: 'app.component.html'
})
 
export class AppComponent implements OnInit {
    registerForm: FormGroup;
    submitted = false;
 
    constructor(private formBuilder: FormBuilder) { }
 
    ngOnInit() {
        this.registerForm = this.formBuilder.group({
            firstName: ['', Validators.required],
            lastName: ['', Validators.required],
            email: ['', [Validators.required, Validators.email]],
            password: ['', [Validators.required, Validators.minLength(6)]]
        });
    }
 
    // convenience getter for easy access to form fields
    get f() { return this.registerForm.controls; }
 
    onSubmit() {
        this.submitted = true;
 
        // stop here if form is invalid
        if (this.registerForm.invalid) {
            return;
        }
 
        alert('SUCCESS!! :-)')
    }
}
```

In the template
```html
<div class="jumbotron">
    <div class="container">
        <div class="row">
            <div class="col-md-6 offset-md-3">
                <h2>Angular 6 Reactive Form Validation</h2>
                <form [formGroup]="registerForm" (ngSubmit)="onSubmit()">
                    <div class="form-group">
                        <label>First Name</label>
                        <input type="text" formControlName="firstName" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.firstName.errors }" />
                        <div *ngIf="submitted && f.firstName.errors" class="invalid-feedback">
                            <div *ngIf="f.firstName.errors.required">First Name is required</div>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Last Name</label>
                        <input type="text" formControlName="lastName" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.lastName.errors }" />
                        <div *ngIf="submitted && f.lastName.errors" class="invalid-feedback">
                            <div *ngIf="f.lastName.errors.required">Last Name is required</div>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Email</label>
                        <input type="text" formControlName="email" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.email.errors }" />
                        <div *ngIf="submitted && f.email.errors" class="invalid-feedback">
                            <div *ngIf="f.email.errors.required">Email is required</div>
                            <div *ngIf="f.email.errors.email">Email must be a valid email address</div>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Password</label>
                        <input type="password" formControlName="password" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.password.errors }" />
                        <div *ngIf="submitted && f.password.errors" class="invalid-feedback">
                            <div *ngIf="f.password.errors.required">Password is required</div>
                            <div *ngIf="f.password.errors.minlength">Password must be at least 6 characters</div>
                        </div>
                    </div>
                    <div class="form-group">
                        <button [disabled]="loading" class="btn btn-primary">Register</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
```
### Working with multiple checkboxes
Define the checkbox values

```javascript
public signupForm: FormGroup;
alerts = ['News', 'Movies', 'Sports'];
```

Define the form elements
```javascript
this.signupForm = this.fb.group({
  emailAlerts: this.fb.array([])
});
```

Loop through the checkbox values and render in html
```html
<span *ngFor="let alert of alerts; let i=index"> 
  <input type="checkbox" (change)="onChangeAlerts(alert, $event.target.checked)">{{alert}}
</span>
```
Create the change event function in component
```javascript
onChangeAlerts(alert: string, isChecked: boolean) {
  const formArr = <FormArray>this.signupForm.controls.emailAlerts;

  if (isChecked) {
    formArr.push(new FormControl(alert));
  } else {
    let index = formArr.controls.findIndex(x => x.value == alert)
    formArr.removeAt(index);
  }
}
```

## Animations

### Simple Animation
First step is to uncomment the "web-animations" polyfill in "polyfills.ts" file

```javascript
import 'web-animations-js';  // Run `npm install --save web-animations-js`.
```
As the instruction says, next we have to execute the command `npm install --save web-animations-js`

Once the npm package is installed add the "BrowserAnimationsModule" in the "app.module.ts"

```javascript
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';
```
and then import it to the "imports" array

```javascript
imports: [
  BrowserModule,
  ReactiveFormsModule,
  BrowserAnimationsModule
],
```

Once this is done, open the component file and import the following:

```javascript
import { trigger, animate, style, transition } from '@angular/animations';
```

In the component decorator add the "animations" property like following to have a simple fadein/fadeout animation:

```javascript
@Component({
  selector: 'app-message',
  templateUrl: './message.component.html',
  styleUrls: ['./message.component.css'],
  animations: [
    trigger('fadeIn', [
      transition('void => *', [
        style({opacity: 0}),
        animate('5000ms ease-in-out', style({opacity: 1}))
      ]),
      transition('* => void', [
        style({opacity: 1}),
        animate('5000ms ease-in-out', style({opacity: 0}))
      ])
    ]),
  ]
})
```
For triggering the animation we have to use the animation trigger in the html template

```html
<div [@fadeIn]>
  Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ipsam, laudantium soluta quaerat nihil, totam voluptatum ad deleniti dolore voluptatem minus ducimus temporibus quod cupiditate ex tenetur cumque dolorem alias. Ex?
</div>
```

When the page will be loaded, you will notice a simple fadein animation for the above div.


### Stagger animation
In the component file add the following imports

```javascript
import { trigger, animate, style, transition, query, keyframes, stagger } from '@angular/animations';
```

In the component decorator add the following:

```javascript
@Component({
  selector: 'app-message',
  templateUrl: './message.component.html',
  styleUrls: ['./message.component.css'],
  animations: [

    trigger('todos', [
      transition('* => *', [
        query(':enter', style({ opacity: 0 }), {optional: true}),
        query(':enter', stagger('300ms', [
          animate('.6s ease-in', keyframes([
            style({opacity: 0, transform: 'translateY(-75%)', offset: 0}),
            style({opacity: .5, transform: 'translateY(35px)',  offset: 0.3}),
            style({opacity: 1, transform: 'translateY(0)',     offset: 1.0}),
          ]))]), {optional: true})
      ])
    ])
  ]
})
```

In the html add the animation trigger to the container containing the list items

```html
<div [@todos]="todos.length">
  <div *ngFor="let todo of todos" class="todoblock">
    {{todo}}
  </div>
</div>
```

## Deploying app

We can do a production build using:

```javascript
ng build --prod
```

This builds your application and outputs it to your outDir defined in .angular-cli.json. By default this will be "/dist". You can take the contents of this folder and drop it into the root of your Web server and everything will work just fine.

However if we want to deploy to a sub-folder in our web server, we have to add "--base-href" flag

```javascript
ng build --base-href "/sub-folder/" --prod
```

After deployment, we can access the url as "http://domain/sub-folder"

