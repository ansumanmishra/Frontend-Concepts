# Polymer JS Concepts

## Pros over angular
- Polymer is better for creating progressive webapps
	- Reliable Load instantly and never show the downasaur, even in uncertain network conditions.
	- Fast Respond quickly to user interactions with silky smooth animations and no janky scrolling.
	- Engaging - Feel like a natural app on the device, with an immersive user experience.
	- Scalable
	- Easy to maintain
- Code is cleaner and has all the definations in one place
- Behaviors
	- Polymer uses behaviors to share code between different components
- Styling
	- Mixin declaration (Custom CSS mixin)
	- CSS variables
- Working with Native JavaScript Objects Is Much Easier
- Angular 2 components can be used only in Angular2 applications but Polymer components can ideally be re-used in any web application

## Cons
- Some components don’t have good documentation and examples.
- Browser supports
-  Angular 2 is a full fledged framework. It provides a way to structure the application. It comes with built-in application routing, state management and data communication
- Polymer facilitates only component creation. However, polymer team has created few components which can be used for routing. A separate library can be used to manage data communication. e.g. Redux, or any other Flux based library

## Polymer components vs angular directives
Use Polymer Custom Elements instead of Angular Directives for building components and use Angular Directives just to add behavior to existing components

Angular is a complete framework for building webapps, whereas Polymer is a library for creating Web Components. Those components, however, can then be used to build a webapp.

Polymer is a library for creating Web Components, which are a set of W3C standards and upcoming browser APIs for defining your own custom HTML elements

## Types of Polymer elements
**Iron elements:** iron-ajax, iron-list, iron-icons, iron-pages

**Paper elements:**
- Paper input elements - paper-checkbox, paper-input, paper-listbox
- Paper overlay elements - paper-dialog
- Paper ui elements - paper-item, paper-badge, paper-tabs, paper-toolbar

**Gold elements:** e-commerce-specific use-cases, like checkout flows
- gold-cc-cvc-input: A credit card verification input field
- gold-cc-expiration-input: A field for a credit card expiration date
- gold-cc-input: An input field for a credit card number
- gold-email-input: An input field for an email address
- gold-phone-input: An input field for a phone number
- gold-zip-input: An input field for a zip

**Neon elements:** Animation-related elements

**Platinum elements:** Elements for app-like features, like push notifications, offline caching and bluetooth, platinum-bluetooth

## Lifecycle
- created
- ready
- attached
- detached
- attributeChanged

## Templates
```javascript
<template is="dom-repeat" items="{{employees}}">
    <div># [[index]]</div>
    <div>First name: <span>[[item.first]]</span></div>
    <div>Last name: <span>[[item.last]]</span></div>
</template>
```

Conditional template
```javascript
<template is="dom-if" if="{{condition}}">
  ...
</template>
```

## 2.0 features
**Improved interoperability:** By removing the need to use Polymer.dom for DOM manipulation, Polymer 2.0 makes it easier to use Polymer components with other libraries and frameworks. In addition, the shady DOM code has been separated out into a reusable polyfill, instead of being integrated into Polymer.

**Data system improvements:** Polymer 2.0 includes targeted improvements to the data system. These changes make it easier to reason about and debug the propagation of data through and between elements.

**More standard:** Polymer 2.0 uses standard ES6 classes and the standard custom elements v1 methods for defining elements, instead of a Polymer factory method. You can mix in features using standard JavaScript (class expression mixins) instead of Polymer behaviors. (The Polymer factory method is still supported using a compatibility layer.

## Properties
**type:** Boolean, Date, Number, String, Array or Object
**reflectToAttribute:** true (Tells Polymer to write value to an attribute every time it changes (not good for performance)
**notify:** true (Means to make value two-way bindable from the outside, not from the inside)
**computed:**
**observer:**


## Selectors
this.$$(selector) -  for locating dynamically-created nodes in your element’s local DOM.
Polymer.dom(this.$).querySelector('createButton')
this.$.id

**Others:**
```javascript
Polymer.dom(parent).childNodes
Polymer.dom(parent).children
Polymer.dom(node).parentNode
Polymer.dom(node).firstChild
Polymer.dom(node).lastChild
Polymer.dom(node).firstElementChild
Polymer.dom(node).lastElementChild
Polymer.dom(node).previousSibling
Polymer.dom(node).nextSibling
Polymer.dom(node).textContent
Polymer.dom(node).innerHTML
```
## importHref

Example:
```javascript
this.importHref('path/to/page.html', function(e) {
    // e.target.import is the import document.
}, function(e) {
    // loading error
});
```

## Behavior

Example:
```javascript
Polymer({
  is: 'super-element',
  behaviors: [SuperBehavior]
});

<script>
    HighlightBehavior = {

      properties: {
        isHighlighted: {
          type: Boolean,
          value: false,
          notify: true,
          observer: '_highlightChanged'
        }
      }
    }
}
</script>
```
