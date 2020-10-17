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

## Resource
Secrets of the JavaScript Ninja, Second Edition by John Resig, Bear Bibeault, and Josip Maras
Published by Manning Publications, 2016
