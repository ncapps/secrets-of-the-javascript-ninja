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
1. **Page building—** HTML code is processed to create the DOM, and global JavaScript code is executed when script nodes are encountered. During this execution, the JavaScript code can modify the current DOM to any degree and can even register event handlers—functions that are executed when a particular event occurs (for example, a mouse click or a keyboard press). Registering event handlers is easy: Use the built-in `addEventListener` method.
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
4. Via the function’s apply or call methods—skulk.call(ninja) or skulk.apply(ninja)

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
- Arrow functions don’t have their own `this` value. Instead, they remember the value of the `this` parameter at the time of their *definition*.

**Using the bind method**
- Every function has access to the `bind` method that, in short, creates a new function. This function has the same body, but its context is always bound to a certain object, regardless of the way we invoke it.
- Use the `bind` method, available to all functions, to create a new function that’s always bound to the argument of the bind method. In all other aspects, the bound function behaves as the original function.

**Summary**
- When invoking a function, in addition to the parameters explicitly stated in the function definition, function invocations are passed in two implicit parameters: `arguments` and `this`:
1. The `arguments` parameter is a collection of arguments passed to the function. It has a `length` property that indicates how many arguments were passed in, and it enables us to access the values of arguments that don’t have matching parameters. In nonstrict mode, the arguments object aliases the function parameters (changing the argument changes the value of the parameter, and vice versa). This can be avoided by using strict mode.
2. The `this` parameter represents the function context, an object to which the function invocation is associated. How `this` is determined can depend on the way a function is defined as well as on how it’s invoked.

## Chapter 5. Functions for the master: closures and scopes
- *closures* - a mechanism that allows a function to access all variables that are in scope when the function itself is created.
- closures can help us mimic private variables
- There are two main types of JavaScript code:
1. global code, placed outside all functions
2. function code, contained in functions.
- When our code is being executed by the JavaScript engine, each statement is executed in a certain execution context.
- There’s only one global execution context, created when our JavaScript program starts executing, whereas a new function execution context is created on each function invocation.
- *Function context* is the object on which our function is invoked, which can be accessed through the `this` keyword. Function context is different from function execution context.
- Even though the *execution context stack* is an internal JavaScript concept, you can explore it in any JavaScript debugger, where it’s referred to as a *call stack*
- A *lexical environment* is an internal JavaScript engine construct used to keep track of the mapping from identifiers to specific variables. People often colloquially refer to them as scopes.
- Each execution context has a lexical environment associated with it that contains the mapping for all identifiers defined directly in that context
- Whenever a function is created, a reference to the lexical environment in which the function was created is stored in an internal (meaning that you can’t access or manipulate it directly) property named `[[Environment]]`

**Understanding Types of JavaScript Variables**
- In JavaScript, we can use three keywords for defining variables: var, let, and const. They differ in two aspects: mutability and their relationship toward the lexical environment.
- All variables defined with `const` are immutable, meaning that their value can be set only once.
- Variables defined with keywords `var` and `let` are typical run-of-the-mill variables, whose value we can change as many times as necessary.
- A value of a `const` variable can be set only on initialization and that we can’t assign a completely new value later. We can still modify the existing value; we just can’t completely override it.
- When we use the `var` keyword, the variable is defined in the closest function or global lexical environment. (Note that blocks are ignored!)
- We can use `let` and `const` to define block-scoped, function-scoped, and global-scoped variables.
- `let` and `const` define variables in the closest lexical environment (which can be a block environment, a loop environment, a function environment, or even the global environment).
- Variables and function declarations are technically not “moved” anywhere. They’re visited and registered in lexical environments before any code is executed. Although *hoisting*, as it’s most often defined, is enough to provide a basic understanding of how JavaScript scoping works.

**Summary**
- Closures allow a function to access all variables that are in scope when the function itself was defined. They create a “safety bubble” of the function and the variables that are in scope at the point of the function’s definition. This way, the function has all it needs to execute, even if the scope in which the function was created is long gone.
- We can use function closures for these advanced uses:
1. Mimic private object variables, by closing over constructor variables through method closures
2. Deal with callbacks, in a way that significantly simplifies our code
- JavaScript engines track function execution through an execution context stack (or a call stack). Every time a function is called, a new function execution context is created and placed on the stack. When a function is done executing, the matching execution context is popped from the stack.
- JavaScript engines track identifiers with lexical environments (or colloquially, scopes).
- In JavaScript, we can define globally-scoped, function-scoped, and even-block scoped variables.
- To define variables, we use `var`, `let`, and `const` keywords:
1. The `var` keyword defines a variable in the closest function or global scope (while ignoring blocks).
2. `let` and `const` keywords define a variable in the closest scope (including blocks), allowing us to create block-scoped variables, something that wasn’t possible in pre-ES6 JavaScript.
3. `const` allows us to define “variables” whose value can be assigned only once.
- Closures are merely a side effect of JavaScript scoping rules. A function can be called even when the scope in which it was created is long gone.

## Chapter 6. Functions for the future: generators and promises
- *Generators* are a special type of function. Whereas a standard function produces at most a single value while running its code from start to finish
- Generators produce multiple values, on a per request basis, while suspending their execution between these requests
- A *promise* is a placeholder for a value that we don’t have yet but will at some later point. They’re especially good for working with multiple asynchronous steps.
- Creating a generator function is simple: We append an asterisk (*) after the `function` keyword. This enables us to use the new `yield` keyword within the body of the generator to produce individual values.
- Making a call to a generator doesn’t mean that the body of the generator function will be executed. Instead, an iterator object         is created, an object through which we can communicate with the generator
- The iterator is used to control the execution of the generator. One of the fundamental things that the iterator object exposes         is the `next` method, which we can use to control the generator by requesting a value from it.
- The iterator, created by calling a generator, exposes a `next` method through which we can request a new value from the generator. The `next` method returns an object that carries the value produced by the generator, as well as the information stored in the `done` property that tells us whether the generator has additional values to produce.
- By using the `yield*` operator on an iterator, we yield to another generator
- The layout of a web page is based on the DOM, a tree-like structure of HTML nodes, in which every node, except the root one, has exactly one parent, and can have zero or more children.
- We can achieve two-way communication with a generator. We use `yield` to return data from a generator, and the iterator’s `next()` method to pass data back to the generator.
- The `next` method supplies the value to the waiting `yield` expression, so if there’s no `yield` expression waiting, there’s nothing to supply the value to. For this reason, we can’t supply values over the first call to the `next` method. But remember, if you need to supply an initial value to the generator, you can do so when calling the generator itself
- Calling a generator doesn’t execute it. Instead, it creates a new iterator that we can use to request values from the generator. After a generator produces (or yields) a value, it suspends its execution and waits for the next request. So in a way, a generator works almost like a small program, a state machine that moves between states.
- Generators have to be able to resume their execution. Because the execution of all functions is handled by execution contexts, the iterator keeps a reference to its execution context, so that it’s alive for as long as the iterator needs it.
- Standard functions can only be called anew, and each call creates a *new* execution context. In contrast, the execution context of a generator can be temporarily suspended and resumed at will.
- A *promise* is a placeholder for a value that we don’t have now but will have later; it’s a guarantee that we’ll eventually know the result of an asynchronous computation.
- To create a promise, we use the built-in Promise constructor, to which we pass a function. This function, called an *executor* function, has two parameters: `resolve` and `reject`. The executor is called *immediately* when constructing the Promise object with two built-in functions as arguments: `resolve`, which we manually call if we want the promise to resolve successfully, and `reject`, which we call if an error occurs.
- We use the promise by calling the built-in `then` method on the Promise object, a method to which we pass two callback functions: a success callback and a failure callback. The former is called if the promise is resolved successfully (if the resolve function is called on the promise), and the latter is called if there’s a problem (either an unhandled exception occurs or the reject function is called on a promise).
- Problems with callbacks:
  1. Difficult error handling
  2. Performing sequences of steps is tricky
  3. Performing a number of steps in parallel is tricky
- States of a promise
  - A promise starts in the *pending* state, in which we know nothing about our promised value. That’s why a promise in the pending state is also called an `unresolved` promise.
  - If the promise’s `resolve` function is called, the promise moves into the *fulfilled* state, in which we’ve successfully obtained the promised value.
  - If the promise’s `reject` function is called, or if an unhandled exception occurs during promise handling, the promise moves into the *rejected* state
  - We say that a promise is *resolved* (either successfully or not).
- There are two ways of rejecting a promise: *explicitly*, by calling the passed-in `reject` method in the executor function of a promise, and *implicitly*, if during the handling of a promise, an unhandled exception occurs.
-  We can chain in the `catch` method after the `then` method, to also provide an error callback that will be invoked when a promise gets rejected.

**Summary**
- Generators are functions that generate sequences of values—not all at once, but on a per request basis.
- Unlike standard functions, generators can suspend and resume their execution. After a generator has generated a value, it suspends its execution without blocking the main thread and waits for the next request.
- A generator is declared by putting an asterisk (`*`) after the function keyword. Within the body of the generator, we can use the new `yield` keyword that yields a value and suspends the execution of the generator. If we want to yield to another generator, we use the `yield*` operator.
- Calling a generator creates an iterator object through which we control the execution of the generator. We request new values from the generator by using the iterator’s `next` method, and we can even throw exceptions into the generator by calling the iterator’s `throw` method. In addition, the `next` method can be used to send in values to the generator.
- A promise is a placeholder for the results of a computation; it’s a guarantee that eventually we’ll know the result of the computation, most often an asynchronous computation. A promise can either succeed or fail, and after it has done so, there will be no more changes.
- Promises significantly simplify our dealings with asynchronous tasks. We can easily work with sequences of interdependent asynchronous steps by using the `then` method to chain promises. Parallel handling of multiple asynchronous steps is also greatly simplified; we use the `Promise.all` method.
- We can combine generators and promises to deal with asynchronous tasks with the simplicity of synchronous code

## Chapter 7. Object orientation with prototypes
- Every object can have a reference to its *prototype*, an object to which the search for a particular property can be delegated to, if the object itself doesn’t have that property.
- Every object can have a prototype, and an object’s prototype can also have a prototype, and so on, forming a *prototype chain*.
- The reference between an object and the function’s prototype is established at the time of object instantiation
- By using the `constructor` property, we can access the function that was used to create the object. This information can be used as a form of type checking.
- *Inheritance* is a form of reuse in which new objects have access to properties of existing objects. This helps us avoid the need to repeat code and data across our code base.
- When we perform an `instanceof` operation, we can determine whether the function inherits the functionality of any object in its prototype chain.
- Even though ES6 has introduced the `class` keyword, we’re still dealing with prototypes; classes are syntactic sugar designed to make our lives a bit easier when mimicking classes in JavaScript. These are functionally equivalent
```javascript
// ES5
function Ninja(name) {
  this.name = name;
}
Ninja.prototype.swingSword = function() {
  return true;
};

// ES6
class Ninja {
  constructor(name){
    this.name = name;
  }

  swingSword(){
    return true;
  }
}
```
- We define classes and specify their relationship by using the `extends` keyword

**Summary**
- JavaScript objects are simple collections of named properties with values.
- JavaScript uses prototypes.
- Every object can have a reference to a *prototype*, an object to which we delegate the search for a particular property, if the object itself doesn’t have the searched-for property. An object’s prototype can have its own prototype, and so on, forming a prototype chain.
- We can define the prototype of an object by using the `Object.setPrototypeOf` method.
- Prototypes are closely linked to constructor functions. Every function has a `prototype` property that’s set as the prototype of objects that it instantiates.
- A function’s prototype object has a `constructor` property pointing back to the function itself. This property is accessible to all objects instantiated with that function and, with certain limitations, can be used to find out whether an object was created by a particular function.
- In JavaScript, almost everything can be changed at runtime, including an object’s prototypes and a function’s prototypes
- If we want the instances created by a Ninja constructor function to “inherit” (more accurately, have access to) properties accessible to instances created by the Person constructor function, set the prototype of the Ninja constructor to a new instance of the Person class.
- In JavaScript, properties have attributes (configurable, enumerable, writable). These properties can be defined by using the built-in `Object.defineProperty` method.
- JavaScript ES6 adds support for a `class` keyword that enables us to more easily mimic classes. Behind the scenes, prototypes are still in play
- The `extends` keyword enables elegant inheritance

## Chapter 8. Controlling access to objects
- getter and setter methods can be defined in two ways:
  1. By specifying them within object literals or within ES6 class definitions
  2. By using the built-in `Object.defineProperty` method
- native getters and setters allow us to specify properties that are accessed as standard properties, but that are methods whose execution is triggered immediately when the property is accessed
- JavaScript doesn’t have private object properties. Instead, we can mimic them through closures, by defining variables and specifying object methods that will close over those variables
- getters and setters can be used to define *computed properties*, properties whose value is calculated per request. Computed properties don’t store a value; they provide a get and/or a set method to retrieve and set other properties indirectly.
- If a certain value depends only on the internal state of the object, it makes perfect sense to represent it as a data field, a property, instead of a function.

**Summary**
- We can monitor our objects with getters, setters, and proxies.
- By using accessor methods (getters and setters), we can control access to object properties.
  - Accessor properties can be defined by using the built-in `Object.define-Property` method or with a special get and set syntax as parts of object literals or ES6 classes.
  - A `get` method is implicitly called whenever we try to read, whereas a `set` method is called whenever we assign a value to the matching object’s property
  - Getter methods can be used to define computed properties, properties whose value is calculated on a per request basis, whereas setter methods can be used to achieve data validation and logging.
- Proxies are an ES6 addition to JavaScript and are used to control other objects
  - Proxies enable us to define custom actions that will be executed when an object is interacted with (for example, when a property is read or a function is called)
  - All interactions have to go through the proxy, which has traps that are triggered when a specific action occurs.
- Proxies aren’t fast, so be careful when using them in code that’s executed a lot. We recommend that you do performance testing.

## Chapter 9. Dealing with collections
- There are two fundamental ways to create new arrays:
  1. Using the built-in `Array` constructor
  2. Using array literals []
- Using array literals to create arrays is preferred over creating arrays with the Array constructor. The primary reason is simplicity: `[]` versus `new Array()`
- each array has a `length` property that specifies the size of the array
- we can use the built-in `push` method to append an item to the end of the array
- We can also add new items to the beginning of the array by using the built in `unshift` method
- Calling the built-in `pop` method removes an element from the end of the array
- We can also remove an item from the beginning of the array by using the built-in `shift` method
- `push` and `pop` are significantly faster operations than `shift` and `unshift`, and we recommend using them unless you have a good reason to do otherwise
- `splice` takes two arguments: the index from which the splicing starts, and the number of elements to be removed. In addition, the `splice` method returns an array of items that have been removed
- JavaScript arrays have a built-in `forEach` method
- Creating new arrays based on the items in an existing array is called *mapping an array*. JavaScript has a `map` function that does exactly that.
- The `every` method returns true only if the passed-in callback returns true for every item in the array
- The `some` method calls the callback on each array item until an item is found for which the callback returns a true value
- Use the built-in `find` method to find an array item that satisfies a certain condition
- If we need to find multiple items satisfying a certain criterion, we can use the `filter` method, which creates a new array containing all the items that satisfy that criterion
- Use the built-in `indexOf` method to find the index of a particular item
- Use the `lastIndexOf` method to find the last index of a particular item
- The `findIndex` method takes a callback and returns the index of the first item for which the callback returns `true`
- The JavaScript engine implements the sorting algorithm. The only thing we have to provide is a callback that informs the sorting         algorithm about the relationship between two array items.
- The `reduce` method works by taking the initial value and calling the callback function on each array item with the result of the previous callback invocation (or the initial value) and the current array item as arguments
- **Don't use objects as maps**. For two reasons:
  1. properties inherited through prototypes
  2. support for string-only keys
- Use the `Map` type
- Use `for...of` loop to iterate over maps
- Sets are collections of unique items
- Use the built-in `Set` constructor to create a new set
- Use `for...of` loop to iterate over sets
- A *union* of two sets, A and B, creates a new set that contains all elements from both A and B
- The *intersection* of two sets, A and B, creates a set that contains elements of A that are also in B
- The *difference* of two sets, A and B, contains all elements that are in set A but are not in set B

**Summary**
- Arrays are a special type of object with a `length` property and `Array.prototype` as their prototype
- We can create new arrays using the array literal notation (`[]`) or by calling the built-in `Array` constructor
- We can modify the contents of an array using several methods accessible from array objects:
  1. The built-in `push` and `pop` methods add items to and remove items from the end of the array.
  2. The built-in `shift` and `unshift` methods add items to and remove items from the beginning of the array.
  3. The built-in `splice` method can be used to remove items from and add items to arbitrary array positions.
- All arrays have access to a number of useful methods:
  1. The `map` method creates a new array with the results of calling a callback on every element.
  2. The `every` and some methods determine whether all or some array items satisfy a certain criterion.
  3. The `find` and `filter` methods find array items that satisfy a certain condition.
  4. The `sort` method sorts an array.
  5. The `reduce` method aggregates all items in an array into a single value.
- You can reuse the built-in array methods when implementing your own objects by explicitly setting the method call context with the `call` or `apply` method.
- Maps and dictionaries are objects that contain mappings between a key and a value
- Objects in JavaScript are lousy maps because you can only use string values as keys and because there’s always the risk of            accessing prototype properties. Instead, use the new built-in `Map` collection

## Chapter 10. Wrangling regular expressions
- A regular expression is a way to express a *pattern* for matching strings of text
- Regular expressions require a lot of practice. Here are some resources:
  - http://jsbin.com
  - www.regexplanet.com/advanced/javascript/index.html
  - www.regex101.com/#javascript
- In JavaScript, as with most other object types, we have two ways to create a regular expression:
  1. Via a regular expression literal
  2. By constructing an instance of a RegExp object
  ```javascript
  // regex literal
  const pattern = /test/;

  // RegExp instance
  const pattern = new RegExp("test");
  ```
  - The literal syntax is preferred when the regex is known at development time, and the constructor approach is used when the regex is constructed at runtime by building it up dynamically in a string.
- five flags can be associated with a regex:
  1. `i`— Makes the regex case-insensitive, so /test/i matches not only test, but also Test, TEST, tEsT
  2. `g`— Matches all instances of the pattern, as opposed to the default of local, which matches only the first occurrence
  3. `m`— Allows matches across multiple lines, as might be obtained from the value of a textarea element
  4. `y`— Enables sticky matching. A regular expression performs sticky matching in the target string by attempting to match from the last match position
  5. `u`— Enables the use of Unicode point escapes (\u{...})
- Predefined character clasess help simplify our regex patterns
- Preconstructing and precompiling regular expressions so that they can be reused (executed) time and time again is a recommended technique that provides performance gains
- The `match` method of a regular expression returns an array of captured values if a match is found, or `null` if no match is found. The array returned by `match` includes the entire match in the first index, and then each subsequent capture following.

**Summary**
- We can create regular expressions with regular expression literals (`/test/`) and with the RegExp constructor (`new RegExp("test")`). Literals are preferred when the regex is known at development time, and the constructor when the regex is constructed at runtime.
- Use `[]` (as in `[abc]`) to specify a set of characters that we wish to match.
- Use `^` to signify that the pattern must appear at the beginning of a string and `$` to signify that the pattern must appear at the end of a string.
- Use `?` to specify that a term is optional, `+` that a term should appear one or many times, and `*` to specify that a term appears zero, one, or many times.
- Use `.` to match any character.
- We can use backslash (`\`) to escape special regex characters (such as . [ $ ^).
- Use parentheses `()` to group multiple terms together, and pipe (`|`) to specify alternation
- Portions of a string that are successfully matched against terms can be back referenced with a backslash followed by the number of the capture (`\1`, `\2`, and so on).
- Every string has access to the `match` function, which takes in a regular expression and returns an array containing the entire matched string along with any matched captures. We can also use the `replace` function, which causes a replacement on pattern matches rather than on a fixed string.

## Resource
Secrets of the JavaScript Ninja, Second Edition by John Resig, Bear Bibeault, and Josip Maras
Published by Manning Publications, 2016
