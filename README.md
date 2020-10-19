# Notes from Secrets of the JavaScript Ninja, Second Edition

## Chapter 1. JavaScript is everywhere
Some JavaScript concepts differ fundamentally from those of most other languages:
1. Functions are first-class objects
2. Function closures
3. Scopes
4. Prototype-based object orientation

These JavaScript features can help you write more elegant and more efficient code:
1. **Generators**, which are functions that can generate multiple values on a per-request basis and can suspend their execution between requests
2. **Promises**, which give us better control over asynchronous code
3. **Proxies**, which allow us to control access to certain objects
4. **Advanced array methods**, which make array-handling code much more elegant
5. **Maps**, which we can use to create dictionary collections; and sets, which allow us to deal with collections of unique items
6. **Regular expressions**, which let us simplify what would otherwise be complicated pieces of code
7. **Modules**, which we can use to break code into smaller, relatively self-contained pieces that make projects more manageable

- Transpilers (“transformation + compiling”), are tools that take cutting-edge JavaScript code and transform it into equivalent (or, if that’s not possible, similar) code that works properly in most current browsers.

The browser provides various concepts and APIs to thoroughly explore:
1. **The Document Object Model (DOM)**— The DOM is a structured representation of the UI of a client-side web application that is, at least initially, built from the HTML code of a web application.
2. **Events**— A huge majority of JavaScript applications are event-driven applications, meaning that most of the code is executed in the context of a response to a particular event. Examples of events include network events, timers, and user-generated events such as clicks, mouse moves, keyboard presses, and so on.
3. **Browser API**— To help us interact with the world, the browser offers an API that allows us to access information about devices, store data locally, or communicate with remote servers.

Development Best Practices:
1. Debugging skills
2. Testing
3. Performance analysis

**Summary**
- Client-side web applications are among the most popular today, and the concepts, tools, and techniques once used only for their development have permeated other application domains. Understanding the foundations of client-side web applications will help you develop applications for a wide variety of domains.
- To improve your development skills, you have to gain a deep understanding of the core mechanics of JavaScript, as well as the infrastructure provided by the browsers.
- JavaScript can be executed in a large number of environments, but the environment where it all began, and the environment we’ll concentrate on, is the browser.
- In addition to JavaScript, we’ll explore browser internals such as the DOM (a structured representation of the web page UI) and events, because client-side web applications are event-driven applications.

## Chapter 2. Building the page at runtime
**Page-building Phase**
- Before a web application can be interacted with or even displayed, the page must be built from the information in the response received from the server (usually HTML, CSS, and JavaScript code)
- It’s important to emphasize that, although the HTML and the DOM are closely linked, with the DOM being constructed from HTML, they aren’t one and the same.
- You should think of the HTML code as a blueprint the browser follows when constructing the initial DOM—the UI—of the page.
- During page construction, the browser can encounter a special type of HTML element, the script element, which is used for including JavaScript code. When this happens, the browser pauses the DOM construction from HTML code and starts executing JavaScript code.
- All JavaScript code contained in the script element is executed by the browser’s JavaScript engine
- The browser provides an API through a global object that can be used by the JavaScript engine to interact with and modify the page.
- The primary global object that the browser exposes to the JavaScript engine is the `window` object, which represents the window in which a page is contained. The `window` object is the global object through which all other global objects, global variables (even user-defined ones), and browser APIs are accessible
- One of the most important properties of the global window object is the `document`, which represents the DOM of the current page. By using this object, the JavaScript code can alter the DOM of the page to any degree, by modifying or removing existing elements, and even by creating and inserting new ones.
- We broadly differentiate between two different types of JavaScript code: *global code* and *function code*.
- When the browser reaches the `script` node in the page-building phase, it pauses the DOM construction based on HTML code and starts executing JavaScript code instead. This means executing the global JavaScript code contained in the `script` element (and functions called by the global code are also executed).
- All user-defined global variables created during the execution of JavaScript code in one `script` element are normally accessible to JavaScript code in other script elements. This happens because the global window object, which stores all global JavaScript variables, is alive and accessible during the entire lifecycle of the page.

**Event-handling Phase**
- The browser execution environment is, at its core, based on the idea that only a single piece of code can be executed at once: the so-called single-threaded execution model.
- All generated events (it doesn’t matter if they’re user-generated, like mouse moves or key presses, or server-generated, such as Ajax events) are placed in the same event queue, in the order in which they’re detected by the browser.
- It’s important to note that the browser mechanism that puts events onto the queue is external to the page-building and event-handling phases. The processing that’s necessary to determine when events have occurred and that pushes them onto the event queue doesn’t participate in the thread that’s handling the events.

The majority of code executes as a result of an event:
1. Browser events, such as when a page is finished loading or when it’s to be unloaded
2. Network events, such as responses coming from the server (Ajax events, server-side events)
3. User events, such as mouse clicks, mouse moves, and key presses
4. Timer events, such as when a timeout expires or an interval fires

Code is set up in advance in order to be executed at a later time. Except for global code, the vast majority of the code we place on a page will execute as the result of some event.

**Registering event handlers**
- Event handlers are functions that we want executed whenever a particular event occurs. In order for this to happen, we have to notify the browser that we’re interested in an event. This is called *event-handler registration*.

There are two ways to register events:
1. By assigning functions to special properties
2. By using the built-in addEventListener method

**Summary**
- The HTML code received by the browser is used as a blueprint for creating the DOM, an internal representation of the structure of a client-side web application.
- We use JavaScript code to dynamically modify the DOM to bring dynamic behavior to web applications.
- The execution of client-side web applications is performed in two phases:
1. **Page building—** HTML code is processed to create the DOM, and global JavaScript code is executed when script nodes are encountered. During this execution, the JavaScript code can modify the current DOM to any degree and can even register event handlers—functions that are executed when a particular event occurs (for example, a mouse click or a keyboard press). Registering event handlers is easy: Use the built-in addEventListener method.
2. **Event handling—** Various events are processed one at a time, in the order in which they were generated. The event-handling phase relies heavily on the event queue, in which all events are stored in the order in which they occurred. The event loop always checks the top of the queue for events, and if an event is found, the matching event-handler function is invoked.

## Chapter 3. First-class functions for the novice: definitions and arguments
- Functions in JavaScript possess all the capabilities of objects and are thus treated like any other object in the language.
- Functions are objects, just with an additional, special capability of being invokable: Functions can be called or invoked in order to perform an action.
- This means that we can pass a function as an argument to another function that might, at a later point in application execution, call the passed-in function. This is an example of a more general concept known as a **callback function**.
- One of the most important features of JavaScript is the ability to create functions in the code anywhere an expression can appear. In addition to making the code more compact and easy to understand (by putting function definitions near where they’re used), this feature can also eliminate the need to pollute the global namespace with unnecessary names when a function isn’t going to be referenced from multiple places within the code.

**Sorting with a comparator**
There’s no need to think about the low-level details of a sorting algorithm (or even which sorting algorithm to choose). We provide a callback that the JavaScript engine will call every time it needs to compare two items.
```javascript
var values = [0, 3, 2, 5, 7, 4, 8, 1];

values.sort(function(value1, value2){
  return value1 - value2;
});
```
**Fun with functions as objects**
1. *Storing functions in a collection*: allows us to easily manage related functions—for example, callbacks that have to be invoked when something of interest occurs.
2. *Memoization* allows the function to remember previously computed values, thereby improving the performance of subsequent invocations. Any sort of caching will certainly sacrifice memory in favor of performance.

**Defining Functions**
The way in which a function is defined significantly influences when the function is available to be invoked and how it behaves, as well as on which object the function can be invoked.
- Function declarations and function expressions:
```javascript
function myFun(){ return 1;}
```
- Arrow functions (often called lambda functions)
```javascript
myArg => myArg*2
```
- Function constructors
```javascript
new Function('a', 'b', 'return a + b')
```
- Generator functions
```javascript
function* myGen(){ yield 1; }
```

- Function declarations and function expressions are the two most common types of functions:
1. Function declarations must have a name, and must be placed as separate statements in our code.
2. Function expressions don’t have to be named, but do have to be a part of another code statement.

**Arrow Function**
- The arrow function definition starts with an optional comma-separated list of parameter names.
- If there are no parameters, or more than one parameter, this list must be enclosed within parentheses.
- If we have only a single parameter, the parentheses are optional.
- This list of parameters is followed by a mandatory fat-arrow operator, which tells us and the JavaScript engine that we’re dealing with an arrow function.

**Arguments and Function Parameters**
- A *parameter* is a variable that we list as part of a function definition.
- An *argument* is a value that we pass to the function when we invoke it.

**Rest parameters**
- By prefixing the last-named argument of a function with an ellipsis (...), we turn it into an array called the *rest parameters*, which contains the remaining passed-in arguments.

**Default parameters**
- To create a default parameter, we assign a value to a function parameter.
- We can assign any values to default parameters: simple, primitive values such as numbers or strings, but also complex types such as objects, arrays, and even functions.
- Moderate use of default parameters—as a means of avoiding null values, or as relatively simple flags that configure the behaviors of our functions—can lead to much simpler and more elegant code.

**Summary**
- Writing sophisticated code hinges upon learning JavaScript as a functional language.
- Functions are first-class objects that are treated just like any other objects within JavaScript. Similar to any other object type, they can be
1. Created via literals
2. Assigned to variables or properties
3. Passed as parameters
4. Returned as function results
5. Assigned properties and methods
- Callback functions are functions that other code will later “call back,” and are often used, especially with event handling.

## Chapter 4. Functions for the journeyman: understanding function invocation
- The implicit function parameters `this` and `arguments`. These are silently passed to functions and can be accessed just like any other explicitly named function parameter within the function’s body.
1. The `this` parameter represents the function context, the object on which our function is invoked.
2. The `arguments` parameter represents all arguments that are passed in through a function call.

- The main point of the arguments object is to allow us to access all arguments that were passed to the function, regardless of whether a particular argument is associated with a function parameter.
- The rest parameter is a real array, which means that we can use all our favorite array methods on it. This gives it a certain advantage over the arguments object.
- The concept of aliasing function parameters through the `arguments` object can be confusing, so JavaScript provides a way to opt out of it by using *strict mode*.
- One of the things that strict mode changes is that it disables `arguments` aliasing.

We can invoke a function in four ways:
1. As a function—skulk(), in which the function is invoked in a straightforward manner
2. As a method—ninja.skulk(), which ties the invocation to an object, enabling object-oriented programming
3. As a constructor—new Ninja(), in which a new object is brought into being
4. Via the function’s apply or call methods—skulk.call(ninja)or skulk.apply(ninja)

**Invocation as a function**
- The function context (the value of the `this` keyword) can be two things:
1. In nonstrict mode, it will be the global context (the window object).
2. In strict mode, it will be undefined.

**Invocation as a method**
- When a function is assigned to a property of an object and the invocation occurs by referencing the function using that property, then the function is invoked as a method of that object.
- When we invoke a function as a method of an object, that object becomes the function context and is available within the function via the this parameter.
- Invoking functions as methods is crucial to writing JavaScript in an object-oriented manner. Doing so enables you to use `this` within any method to reference the method’s “owning” object—a fundamental concept in object-oriented programming.

**Invocation as a constructor**
- There’s nothing special about a function that’s going to be used as a constructor.
- Constructor functions are declared just like any other functions, and we can easily use function declarations and function expressions for constructing new objects.
- A *function constructor* enables us to create functions from dynamically created strings.
- *Constructor functions* are functions that we use to create and initialize object instances.
- When invoked with the keyword `new`, an empty object instance is created and passed to the function as its function context, the `this` parameter.
- If the constructor returns an object, that object is returned as the value of the whole new expression, and the newly constructed object passed as `this` to the constructor is discarded.
- If, however, a nonobject is returned from the constructor, the returned value is ignored, and the newly created object is returned.
- Functions and methods are generally named starting with a verb that describes what they do (skulk, creep, sneak, doSomethingWonderful, and so on) and start with a lowercase letter.
- Constructors, on the other hand, are usually named as a noun that describes the object that’s being constructed and start with an uppercase character: Ninja, Samurai, Emperor, Ronin.

**Invocation with the apply and call methods**
- JavaScript provides a means for us to invoke a function and to explicitly specify any object we want as the function context.
- We do this through the use of one of two methods that exist for every function: `apply` and `call`.
- In the case of `apply`, we use an array of arguments, and in the case of `call`, we list them as call arguments, after the function context
- To facilitate a more functional style, all array objects have access to a `forEach` function that invokes a callback on each element within an array. This is often more succinct, and this style is preferred over the traditional `for` statement by those familiar with functional programming.

**Using arrow functions to get around function contexts**
- Arrow functions don’t have their own `this` value. Instead, they remember the value of the this parameter at the time of their definition.

**Using the bind method**
- Every function has access to the `bind` method that, in short, creates a new function. This function has the same body, but its context is always bound to a certain object, regardless of the way we invoke it.
- Use the `bind` method, available to all functions, to create a new function that’s always bound to the argument of the bind method. In all other aspects, the bound function behaves as the original function.

**Summary**
- When invoking a function, in addition to the parameters explicitly stated in the function definition, function invocations are passed in two implicit parameters: `arguments` and `this`:
1. The `arguments` parameter is a collection of arguments passed to the function. It has a `length` property that indicates how many arguments were passed in, and it enables us to access the values of arguments that don’t have matching parameters. In nonstrict mode, the arguments object aliases the function parameters (changing the argument changes the value of the parameter, and vice versa). This can be avoided by using strict mode.
2. The `this` parameter represents the function context, an object to which the function invocation is associated. How `this` is determined can depend on the way a function is defined as well as on how it’s invoked.


## Resource
Secrets of the JavaScript Ninja, Second Edition by John Resig, Bear Bibeault, and Josip Maras
Published by Manning Publications, 2016
