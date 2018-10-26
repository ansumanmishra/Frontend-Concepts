# React JS Concepts

## What is react js

It's a Javascript library to create user interfaces or components.
It uses the virtual DOM instead of the real DOM
It uses server-side rendering

Advantages:
- It increases the application’s performance
- It can be conveniently used on the client as well as server side

Limitations:
- React is just a library, not a full-blown framework
- Managing states for a complex application can be difficult. So need to use Flux or redux

Virtual DOM
- Server side rendering
- Uni directional data flow

## Difference between React & Angular
Angular is a much fuller featured framework than React. 
vue comes with prebuilt data binding and MVC model, making it way easier to set up compared to react and angular
Angular comes with router, form validations etc. but for React we have to install these. It does not come bundled with the default react package

## Webpack & babel
Webpack is a build tool just like Gulp/Grunt
- creates server
- Create the JS build
- Compiles the es6 code to es5 using Babel

## JSX
JSX is a shorthand for JavaScript XML

```javascript
render(){
    return(        
         <div>
            <h1> Hello World from Edureka!!</h1>
         </div>
    );
}
```

## React Without ES6
With ES6:
```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
Without ES6:
```javascript
var createReactClass = require('create-react-class');
var Greeting = createReactClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
```

## Looping through arrays/objects
```javascript
return userList.map( (user, index) => {
    return <li className="list-group-item" key={index}>{user.name}</li>
});
```

## Virtual DOM
DOM is slow. Every time the DOM changes, browser need to recalculate the CSS, do layout, and repaint the web page. This is what takes time.
So react came up with a concept called as Virtual dom. Its just a copy of the actual DOM. 

Here is how virtual dom works for react:
- Whenever anything may have changed, the entire UI will be re-rendered in a Virtual DOM representation.
- The difference between the previous Virtual DOM representation and the new one will be calculated.
- The real DOM will be updated with what has actually changed. This is very much like applying a patch.

## What are props
Props are simmiler to arguments for pure functions argument. Props of component are passed from parent component which invokes component. Props in a component cannot be modified(Immutable)
	
## What are proptypes
Checks the data types for the props
```javascript
import PropTypes from 'prop-types';
ClassName.propTypes = {
		name: React.PropTypes.string
};
```

```javascript
React.PropTypes.array // Validates whether the passed value is an array
React.PropTypes.bool // Validates whether the passed value is a boolean (true/false).
React.PropTypes.func // Validates whether the passed value is a function
React.PropTypes.number // Validates whether the passed value is a number.
React.PropTypes.object // Validates whether the passed value is an object.
React.PropTypes.string	// Validates whether the passed value is a string.
React.PropTypes.any.isRequired	// Flags the prop as required (Mostly used while using - this.props.children)
```

## How many ways pass data from parent to child:
	
props (this.props.propname)
```html
<div name={"Ansuman"} />
```
children (this.props.children)
```html
<div name={"Ansuman"} />
	<p>Some text goes here </p>
</div>
```

We can pass data from parent to child using props and from child to parent through events

## Communicating between components
Add it here...

## Events
```html
<button onClick={ (e) => this.handleClick(e)}> Click Me!</button>
```

## State

Component state can be modified over time in response to user actions, network responses, and anything. Whenever state changes the react will re-render the component. State is specific to a component

Creating state:

```javascript
import React from 'react';

class App extends React.Component {
   constructor(props) {
      super(props);
		
      this.state = {
         header: "Header from state...",
         content: "Content from state..."
      }
   }
   render() {
      return (
         <div>
            <h1>{this.state.header}</h1>
            <h2>{this.state.content}</h2>
         </div>
      );
   }
}

export default App;
```

Updating state:

```javascript
this.setState({
	header: "New header text"
})
```

## State vs Prop
State is limited to be used inside the component where as props are used to communicate between componenets

## Stateful vs Stateless
Stateless component:

```javascript
import React from 'react';
import './Header.scss';

// Stateless component
const Header = () => {
	return(
		<header>
			<nav className="navbar navbar-default">
				<div className="container-fluid">
					<div className="navbar-header">
						<a className="navbar-brand" href="#">Todo App</a>
					</div>
					<ul className="nav navbar-nav">
						<li className="active"><a href="#">Home</a></li>
					</ul>
				</div>
			</nav>
	    </header>
	);
};

export default Header;
```

Stateful Component:

```javascript
import React from 'react';
class Content extends React.Component {
  constructor(props) {
    super(props);
  }

  deleteTodo(itemId) {
    this.props.deleteTodo(itemId);
  }

  render() {
    this.todoList = this.props.todoList.map((item, i) => {
      return <TodoItem itemName={item.name} itemId={item.id} key={item.id} deleteTodo={this.deleteTodo.bind(this)} />;
    });

    return (
      <div>
        <ul>{this.todoList}</ul>
      </div>
    );
  }
}

export default Content;			
```

## Explain Two way data binding
Add it here...

## Refs & DOM
Refs: Refs provide a way to access DOM nodes or React elements created in the render method

When to Use Refs:
- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

Creating Refs:
```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  focusTextInput() {
    this.textInput.current.focus();
  }
  render() {
    return (
    	<div>
    		<input type="text" ref={this.textInput} />
    		<input type="button" value="Focus the text input" onClick={this.focusTextInput} />
    	</div>
    )
  }
}
```
Accessing Refs:
```javascript
this.myRef.current;
```
	
## Component life cycle
There are 3 phases of a react js component life cycle
- Initial Rendering Phase: This is the phase when the component is about to start its life journey and make its way to the DOM.
- Updating Phase: Once the component gets added to the DOM, it can potentially update and re-render only when a prop or state change occurs. That happens only in this phase.
- Unmounting Phase: This is the final phase of a component’s life cycle in which the component is destroyed and removed from the DOM.

Life cycle methods:
- componentWillMount() – Executed just before rendering takes place both on the client as well as server-side.
- componentDidMount() – Executed on the client side only after the first render.
- componentWillReceiveProps(nextProps) – Invoked as soon as the props are received from the parent class and before another render is called.
- shouldComponentUpdate(nextProps, nextState, nextContext) – Returns true or false value based on certain conditions. If you want your component to update, return true else return false. By default, it returns false.
- componentWillUpdate(nextProps, nextState) – Called just before rendering takes place in the DOM.
- componentDidUpdate(prevProps, prevState, prevContext) – Called immediately after rendering takes place.
- componentWillUnmount() – Called after the component is unmounted from the DOM. It is used to clear up the memory spaces.

## What are Higher Order Components(HOC)
Its brought in from functional programming world. 
**Higher order functions:** When one function returns another function, we call it as higher order functions.

Similariliy a component when returns another component is called Higher order components.

e.g.
```javascript
const PropsLogger = (WappedComponent) => {
	class extends Component {
		render() {
			return <WrappedComponent />
		}
	}
}
```

**Advantages**
- HOC can be used for many tasks like:
- Code reuse, logic and bootstrap abstraction
- Render High jacking
- State abstraction and manipulation
- Props manipulation

## What are Pure Components?

Calling setState calls re-rendering. its kind of blind. It does not look for whether the values have changed or not. sometimes these updates/rerendering are not needed. 

This can be avoided using:
```javascript
shouldComponentUpdate(nextProp, nextState) {
	return ( this.state.val === nextState.val ? false : true)
}
```

This can be handled in a better way using pure components:
```javascript
import {pureComponent} from 'react';
class App extends PureComponent {

}
```

## What is the significance of keys in React?
Keys are used for identifying unique Virtual DOM Elements with their corresponding data driving the UI. They help React to optimize the rendering by recycling all the existing elements in the DOM. These keys must be a unique number or string, using which React just reorders the elements instead of re-rendering them. This leads to increase in application’s performance.

## Router
```javascript
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
<Router>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
  <Route path="/topics" component={Topics} />
</Router>
```
## Router navigation & parameters
```html
<ul>
    <li>
      <Link to="/">Home</Link>
    </li>
    <li>
      <Link to="/about">About</Link>
    </li>
    <li>
      <Link to="/topics">Topics</Link>
    </li>
  </ul>
```
Getting the parameter value: **this.props.params.paramName**

## Why is switch keyword used in React Router v4?

Although a *div* is used to encapsulate multiple routes inside the Router. The ‘switch’ keyword is used when you want to display only a single route to be rendered amongst the several defined routes. The *switch* tag when in use matches the typed URL with the defined routes in sequential order. When the first match is found, it renders the specified route. Thereby bypassing the remaining routes.
```javascript
<switch>
   <route exact path=’/’ component={Home}/>
   <route path=’/posts/:id’ component={Newpost}/>
   <route path=’/posts’   component={Post}/>
</switch>
```

## Flux architecture 
Flux is an architectural pattern which enforces the uni-directional data flow. It controls derived data and enables communication between multiple components using a central Store which has authority for all data. Any update in data throughout the application must occur here only. Flux provides stability to the application and reduces run-time errors.

## How Redux works
Redux is one of the hottest libraries for front end development in today’s marketplace. It is a predictable state container for JavaScript applications and is used for the entire applications state management. Applications developed with Redux are easy to test and can run in different environments showing consistent behavior.

**Store**
A store is a JavaScript object which can hold the application’s state and provide a few helper methods to access the state, dispatch actions and register listeners. The entire state/ object tree of an application is saved in a single store. As a result of this, Redux is very simple and predictable. We can pass middleware to the store to handle processing of data as well as to keep a log of various actions that change the state of stores. All the actions return a new state via reducers.

**Provider**
**Containers**
**Components**
**Actions/Action Creators**
**Reducers**
Reducers are pure functions which specify how the application’s state changes in response to an ACTION. Reducers work by taking in the previous state and action, and then it returns a new state. It determines what sort of update needs to be done based on the type of the action, and then returns new values. It returns the previous state as it is, if no work needs to be done.

**Create store:**
```javascript
import {createStore, applyMiddleware} from 'redux';
import thunk from 'redux-thunk';
import allReducer from './reducers.js';

const store = createStore(
    allReducer,
    applyMiddleware(thunk)
);
```

**Creating reducer:**
```javascript
const userReducer = (state="", action) => {
    switch (action.type) {
        case "ADD_USER":
            return [
                ...state,
                {
                    'name': action.payload.name,
                    'place': action.payload.place,
                    'id': action.payload.id
                }
            ]
        break;

        case "DELETE_USER":
            return state.filter(userList =>
                userList.id !== parseInt(action.payload)
            )
        break;

        case "RESOLVED_GET_USERS":
            return action.data;
        break;
    }
    return state;
};
```

**Combining reducer:**
```javascript
import {combineReducers} from  'redux';
import userReducer from './user/user_reducer';
import postReducer from './post/post_reducer';

const allReducers = combineReducers({
    users: userReducer,
    posts: postReducer,
    profile: profileReducer
});

export default allReducers;
```

**Creating Provider:**
```javascript
import {Provider} from 'react-redux';
ReactDOM.render(
    <Provider store={store}>
            <div>
                <App />
            </div>
    </Provider>,
    document.getElementById('root')
);
```

**Creating Container:**
```javascript
import {connect} from 'react-redux';
import {deleteUser, getUsers, addUser} from './user_actions';

import Listuser from './listuser_component';

const mapStateToProps = (state) => {
    return {
        users: state.users
    };
}

const mapDispatch = {deleteUser, getUsers, addUser};

export default connect(mapStateToProps, mapDispatch)(Listuser);	
```

**Creating Actions/Action creators:**
```javascript
import axios from 'axios';

export function addUser(user) {
    return {
        type: 'ADD_USER',
        payload: user
    }
}

export function getUsers() {
    return (dispatch) => {
        // axios.get('http://localhost:4000/users')
        //     .then(function(response) {
        //         dispatch(resolvedGetUsers(response.data))
        //     })
        //     .catch(function(error) {

        //     });
        fetch('http://localhost:4000/users')
        .then(response => response.json())
        .then(json => dispatch(resolvedGetUsers(json)))
    }
}

function resolvedGetUsers(data) {
  return {
    type: 'RESOLVED_GET_USERS',
    data: data
  } 
}
```

## Redux advantages
**Predictability of outcome:** Since there is always one source of truth, i.e. the store, there is no confusion about how to sync the current state with actions and other parts of the application.

**Maintainability:** It is simple to maintain with a strict structure and predictable outcome.

**Developer tools:** From actions to state changes, developers can track everything going on in the application in real time. There is a chrome extension for this

**Community and ecosystem:** Redux has a huge community behind it which makes it even more captivating to use. A large community of talented individuals contribute to the betterment of the library and develop various applications with it.

**Ease of testing:** Redux’s code is mostly functions which are small, pure and isolated. This makes the code testable and independent.

**Organization:** Redux is precise about how code should be organized, this makes the code more consistent and easier when a team works with it.

## How is Redux different from Flux?
**Flux**	
- The Store contains state and change logic	
- There are multiple stores	
- All the stores are disconnected and flat	
- Has singleton dispatcher	
- React components subscribe to the store	
- State is mutable	

**Redux**
- Store and change logic are separate
- There is only one store
- Single store with hierarchical reducers
- No concept of dispatcher
- Container components utilize connect
- State is immutable

## Fragment
Used to create multiple tags in same parent position which is not possible in react

```javascript
return (
	<Fragment>
		<div>hi</div>
		<div>hello</div>
	</Fragment>
)
```

## What are synthetic events in React?
Synthetic events are the objects which act as a cross-browser wrapper around the browser’s native event. They combine the behavior of different browsers into one API. This is done to make sure that the events show consistent properties across different browsers

## React Native
Used for building native mobile apps using JavaScript and React. It uses the same design as React, letting you compose a rich mobile UI from declarative components.

```javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class WhyReactNativeIsSoGreat extends Component {
  render() {
    return (
      <View>
        <Text>
          If you like React on the web, you will like React Native.
        </Text>
        <Text>
          You just use native components like 'View' and 'Text',
          instead of web components like 'div' and 'span'.
        </Text>
      </View>
    );
  }
}
```
