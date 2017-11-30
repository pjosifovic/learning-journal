![cf](http://i.imgur.com/7v5ASc8.png) Day 03: Asynchronous Callbacks
=====================================

## Introduction

* As a assignment for 401d19 class, I'm starting writing learning journal in which I will be putting stuff that I learned today, issues that I've encounter as well as some short snippets of code, useful links or resources from Slack channels or fellow students. Also, here are some cheatsheets for Markdown [Markdown cheets](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#links)

### Hoisting and Scope

* When the JavaScript runtime (Chrome V8) executes your code, it first reorganizes what you have written so that all variable and function definitions are at the top of their current function scope.

>important thing to know is that function declaration take precedence over variable declaration.

  * **Example:**
  ```JavaScript
    var x = function() {
      console.log(y);
      var y = 1;
    }

    x();
  ```
In this example we can see difference between hoisting and initialization. `console.log(y)` gives `undefined` because definition of `y` was hoisted to the top but it was defined with the `var y = 1`.

* **Example:**
```JavaScript
  var y = 2
  var x = function() {
    console.log(y);
    var y = 1;
  }

  x();
```    
* If we have defined variable `y` outside of function `x`, console.log value of will still be `undefined` because of the function scope.

* Every function invocation has both a scope and a context associated with it.

  * Scope is function-based while context is object-based. In other words, scope pertains to the variable access of a function when it is invoked and is unique to each invocation.

  * Context is always the value of the this keyword which is a reference to the object that “owns” the currently executing code.

* More about understanding Scope and Context [Undestand Scope] (http://ryanmorr.com/understanding-scope-and-context-in-javascript/)

### Concurrency

* Program can run more than one thing at a timeout.

* NodeJs concurrency through the use of events and callbacks
  * When a JavaScript function makes a call to Node APIs, it passes the Node API a callback.
    * The Node api does it thing
    * Passes its results into the callback
    * Enqueues the callback onto the callback queue

### Node asynchronous callback pattern

* Node functions that have asynchronous input or output take a callback as the last argument

  * Node functions that do not pass back data always have callback functions take the form `(error) => { }`
  * Node functions that do pass back data always have callback functions take the form `(error, data) => { }`

  * **Example: The Asyncronous Method**
  ```JavaScript
  function strictAddition(x, y, callback) {
    if(typeof x !== 'number') {
      callback( new Error('First argument is not a number') );
      return;
    }
    if(typeof y !== 'number') {
      callback( new Error('Second argument is not a number') );
      return;
    }
    var result = x + y;
    setTimeout(function() {
      callback(null, result);
    }, 500);  
  };
  ```

  * **Example: The Callback**
  ```JavaScript
  function callback(err, data) {
    if(err) {
      console.log(err);
      return;
    }
    console.log(data);
  };
  ```
  * Running `strictAddition(2, 10, callback); //12` or with an Error `strictAddition('2', 10, callback); // Error = "Second argument is not a number". `



### File System module
  * is Node interface to the file system. It has synchronous and asynchronous methods but for 401d19 class, we should always use asynchronous ones.

  * Useful globals are:
    * `__dirname` - the ABSOLUTE path to the directory the current file is in.
    * Read File
      * `fs.readFile(filepath [, options], callback);`
      * The callback should take the form `(error, data) => {}`
        If `options` is a string, then it specifies the encoding - like `utf8` or `base64` or `hex`.
