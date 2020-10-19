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

## Resource
Secrets of the JavaScript Ninja, Second Edition by John Resig, Bear Bibeault, and Josip Maras
Published by Manning Publications, 2016
