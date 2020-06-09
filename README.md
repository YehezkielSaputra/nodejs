## Node.js 

## Table of Contents

|  No  |  Questions       |
|------|------------------|
| 01. |[What is Node.js?](#01-what-is-nodejs)|
| 02. |[What is Node.js Process Model?](#02-what-is-nodejs-process-model)|
| 03. |[What are the data types in Node.js?](#03-what-are-the-data-types-in-nodejs)|
| 04. |[How to create a simple server in Node.js that returns Hello World?](#04-how-to-create-a-simple-server-in-nodejs-that-returns-hello-world)|
| 05. |[How do Node.js works?](#05-how-do-nodejs-works)|
| 06. |[What is an error-first callback?](#06-what-is-an-error-first-callback)|
| 07. |[What is callback hell in Node.js?](#07-what-is-callback-hell-in-nodejs)|
| 08. |[What are Promises in Node.js?](#08-what-are-promises-in-nodejs)|
| 09. |[What tools can be used to assure consistent style?](#09-what-tools-can-be-used-to-assure-consistent-style)|
| 10. |[When should you npm and when yarn?](#10-when-should-you-npm-and-when-yarn)|
| 11. |[What does the runtime environment mean in Node.js?](#11-what-does-the-runtime-environment-mean-in-nodejs)|
| 12. |[What is a stub?](#12-what-is-a-stub)|
| 13. |[What is a test pyramid?](#13-what-is-a-test-pyramid)|
| 14. |[How can you secure your HTTP cookies against XSS attacks?](#14-how-can-you-secure-your-http-cookies-against-xss-attacks)|
| 15. |[How can you make sure your dependencies are safe?](#15-how-can-you-make-sure-your-dependencies-are-safe)|
| 16. |[What is Event loop in Node.js?](#16-what-is-event-loop-in-nodejs)|
| 17. |[What is REPL?](#17-what-is-repl)|
| 18. |[What is the difference between Asynchronous and Non-blocking?](#18-what-is-the-difference-between-asynchronous-and-non-blocking)|
| 19. |[How to debug an application in Node.js?](#19-how-to-debug-an-application-in-nodejs)|
| 20. |[What are some of the most popular modules of Node.js?](#20-what-are-some-of-the-most-popular-modules-of-nodejs)|

#### 01. ***What is Node.js?***
Node.js is an open-source server side runtime environment built on Chrome's V8 JavaScript engine. It provides an event driven, non-blocking (asynchronous) I/O and cross-platform runtime environment for building highly scalable server-side applications using JavaScript. 

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 02. ***What is Node.js Process Model?***
Node.js runs in a single process and the application code runs in a single thread and thereby needs less resources than other platforms. All the user requests to your web application will be handled by a single thread and all the I/O work or long running job is performed asynchronously for a particular request. So, this single thread doesn't have to wait for the request to complete and is free to handle the next request. When asynchronous I/O work completes then it processes the request further and sends the response.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 03. ***What are the data types in Node.js?***
**Primitive Types**  
* String 
* Number 
* Boolean 
* Undefined 
* Null 
* RegExp 
* **Buffer**: Node.js includes an additional data type called Buffer (not available in browser's JavaScript). Buffer is mainly used to store binary data, while reading from a file or receiving packets over the network.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 04. ***How to create a simple server in Node.js that returns Hello World?***
**Step 01**: Create a project directory
```
cmd> mkdir myapp
cmd> cd myapp
```
**Step 02**: Initialize project and link it to npm
```
npm init
```
This creates a `package.json` file in your myapp folder. The file contains references for all npm packages you have downloaded to your project. The command will prompt you to enter a number of things.
You can enter your way through all of them EXCEPT this one:
```
entry point: (index.js) 
```
Rename this to:
```
app.js
```
**Step 03**: Install Express in the myapp directory
```
npm install express --save
```
**Step 04**: app.js
```javascript
var express = require('express');
var app = express();
app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```
**Step 05**: Run the app
```
node app.js
``` 

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 05. ***How do Node.js works?***
Node is completely event-driven. Basically the server consists of one thread processing one event after another.

A new request coming in is one kind of event. The server starts processing it and when there is a blocking IO operation, it does not wait until it completes and instead registers a callback function. The server then immediately starts to process another event (maybe another request). When the IO operation is finished, that is another kind of event, and the server will process it (i.e. continue working on the request) by executing the callback as soon as it has time.

So the server never needs to create additional threads or switch between threads, which means it has very little overhead. If you want to make full use of multiple hardware cores, you just start multiple instances of node.js

Node JS Platform does not follow Request/Response Multi-Threaded Stateless Model. It follows Single Threaded with Event Loop Model. Node JS Processing model mainly based on Javascript Event based model with Javascript callback mechanism.  
  
**Single Threaded Event Loop Model Processing Steps:**    
* Clients Send request to Web Server.
* Node JS Web Server internally maintains a Limited Thread pool to provide services to the Client Requests.
* Node JS Web Server receives those requests and places them into a Queue. It is known as “Event Queue”.
* Node JS Web Server internally has a Component, known as “Event Loop”. Why it got this name is that it uses indefinite loop to receive requests and process them. 
* Event Loop uses Single Thread only. It is main heart of Node JS Platform Processing Model.
* Even Loop checks any Client Request is placed in Event Queue. If no, then wait for incoming requests for indefinitely.
* If yes, then pick up one Client Request from Event Queue
    * Starts process that Client Request
    * If that Client Request Does Not requires any Blocking IO Operations, then process everything, prepare response and send it back to client.
    * If that Client Request requires some Blocking IO Operations like interacting with Database, File System, External Services then it will follow different approach
        * Checks Threads availability from Internal Thread Pool
        * Picks up one Thread and assign this Client Request to that thread.
        * That Thread is responsible for taking that request, process it, perform Blocking IO operations, prepare response and send it back to the Event Loop
        * Event Loop in turn, sends that Response to the respective Client.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 06. ***What is an error-first callback?***
The pattern used across all the asynchronous methods in Node.js is called *Error-first Callback*. Here is an example:
```javascript
fs.readFile( "file.json", function ( err, data ) {
  if ( err ) {
    console.error( err );
  }
  console.log( data );
});
```
Any asynchronous method expects one of the arguments to be a callback. The full callback argument list depends on the caller method, but the first argument is always an error object or null. When we go for the asynchronous method, an exception thrown during function execution cannot be detected in a try/catch statement. The event happens after the JavaScript engine leaves the try block. 

In the preceding example, if any exception is thrown during the reading of the file, it lands on the callback function as the first and mandatory parameter.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 07. ***What is callback hell in Node.js?***
`Callback hell` is a phenomenon that afflicts a JavaScript developer when he tries to execute multiple asynchronous operations one after the other.

An asynchronous function is one where some external activity must complete before a result can be processed; it is “asynchronous” in the sense that there is an unpredictable amount of time before a result becomes available. Such functions require a callback function to handle errors and process the result.
```javascript
getData(function(a){
    getMoreData(a, function(b){
        getMoreData(b, function(c){ 
            getMoreData(c, function(d){ 
	            getMoreData(d, function(e){ 
		            ...
		        });
	        });
        });
    });
});
```
**Techniques for avoiding callback hell**    

1. Using Async.js
1. Using Promises
1. Using Async-Await
* **Managing callbacks using Async.js**  
`Async` is a really powerful npm module for managing asynchronous nature of JavaScript. Along with Node.js, it also works for JavaScript written for browsers.

Async provides lots of powerful utilities to work with asynchronous processes under different scenarios.
```
npm install --save async
```
* **ASYNC WATERFALL**  
```javascript
var async = require('async');
async.waterfall([
    function(callback) {
        //doSomething
        callback(null, paramx); //paramx will be availaible as the first parameter to the next function
        /**
            The 1st parameter passed in callback.
            @null or @undefined or @false control moves to the next function
            in the array
            if @true or @string the control is immedeatly moved
            to the final callback fucntion
            rest of the functions in the array
            would not be executed
        */
    },
    function(arg1, callback) {
        //doSomething else
      // arg1 now equals paramx
        callback(null, result);
    },
    function(arg1, callback) {
        //do More
        // arg1 now equals 'result'
        callback(null, 'done');
    },
    function(arg1, callback) {
        //even more
        // arg1 now equals 'done'
        callback(null, 'done');
    }
], function (err, result) {
    //final callback function
    //finally do something when all function are done.
    // result now equals 'done'
});
```
* **ASYNC SERIES**  
```javascript
var async = require('async');
async.series([
    function(callback){
        // do some stuff ...
        callback(null, 'one');
        /**
            The 1st parameter passed in callback.
            @null or @undefined or @false control moves to the next function
            in the array
            if @true or @string the control is immedeatly moved
            to the final callback fucntion with the value of err same as
            passed over here and
            rest of the functions in the array
            would not be executed
        */
    },
    function(callback){
        // do some more stuff ...
        callback(null, 'two');
    }
],
// optional callback
function(err, results){
    // results is now equal to ['one', 'two']
});
```
* **Managing callbacks hell using promises**  
Promises are alternative to callbacks while dealing with asynchronous code. Promises return the value of the result or an error exception. The core of the promises is the `.then()` function, which waits for the promise object to be returned. The `.then()` function takes two optional functions as arguments and depending on the state of the promise only one will ever be called. The first function is called when the promise if fulfilled (A successful result). The second function is called when the promise is rejected.

```javascript
var outputPromise = getInputPromise().then(function (input) {
    //handle success
}, function (error) {
    //handle error
});
```
* **Using Async Await**  
Async await makes asynchronous code look like it’s synchronous. This has only been possible because of the reintroduction of promises into node.js. Async-Await only works with functions that return a promise.
```javascript
const getrandomnumber = function(){
    return new Promise((resolve, reject)=>{
        setTimeout(() => {
            resolve(Math.floor(Math.random() * 20));
        }, 1000);
    });
}

const addRandomNumber = async function(){
    const sum = await getrandomnumber() + await getrandomnumber();
    console.log(sum);
}

addRandomNumber();
```

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 08. ***What are Promises in Node.js?***
It allows to associate handlers to an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of the final value, the asynchronous method returns a promise for the value at some point in the future.

Promises in node.js promised to do some work and then had separate callbacks that would be executed for success and failure as well as handling timeouts. Another way to think of promises in node.js was that they were emitters that could emit only two events: success and error.The cool thing about promises is you can combine them into dependency chains (do Promise C only when Promise A and Promise B complete).

The core idea behind promises is that a promise represents the result of an asynchronous operation. A promise is in one of three different states:

* pending - The initial state of a promise.
* fulfilled - The state of a promise representing a successful operation.
* rejected - The state of a promise representing a failed operation.
Once a promise is fulfilled or rejected, it is immutable (i.e. it can never change again).  

**Creating a Promise**
```javascript
var myPromise = new Promise(function(resolve, reject){
   ....
})
```

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 09. ***What tools can be used to assure consistent style?***
* ESLint
* Standard

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 10. ***When should you npm and when yarn?***
* **npm**  
It is the default method for managing packages in the Node.js runtime environment. It relies upon a command line client and a database made up of public and premium packages known as the the npm registry. Users can access the registry via the client and browse the many packages available through the npm website. Both npm and its registry are managed by npm, Inc.
```
node -v
npm -v
```

* **Yarn**  
Yarn was developed by Facebook in attempt to resolve some of npm’s shortcomings. Yarn isn’t technically a replacement for npm since it relies on modules from the npm registry. Think of Yarn as a new installer that still relies upon the same npm structure. The registry itself hasn’t changed, but the installation method is different. Since Yarn gives you access to the same packages as npm, moving from npm to Yarn doesn’t require you to make any changes to your workflow.
```
npm install yarn --global
```

**Comparing Yarn vs npm**    
* Fast: Yarn caches every package it downloads so it never needs to again. It also parallelizes operations to maximize resource utilization so install times are faster than ever.
* Reliable: Using a detailed, but concise, lockfile format, and a deterministic algorithm for installs, Yarn is able to guarantee that an install that worked on one system will work exactly the same way on any other system.
* Secure: Yarn uses checksums to verify the integrity of every installed package before its code is executed.
* Offline Mode: If you've installed a package before, you can install it again without any internet connection.
* Deterministic: The same dependencies will be installed the same exact way across every machine regardless of install order.
* Network Performance: Yarn efficiently queues up requests and avoids request waterfalls in order to maximize network utilization.
* Multiple Registries: Install any package from either npm or Bower and keep your package workflow the same.
* Network Resilience: A single request failing won't cause an install to fail. Requests are retried upon failure.
* Flat Mode: Resolve mismatching versions of dependencies to a single version to avoid creating duplicates.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 11. ***What does the runtime environment mean in Node.js?***
The Node.js runtime is the software stack responsible for installing your web service's code and its dependencies and running your service.

The Node.js runtime for App Engine in the standard environment is declared in the `app.yaml` file:
```javascript
runtime: nodejs10
```

The runtime environment is literally just the environment your application is running in. This can be used to describe both the hardware and the software that is running your application. How much RAM, what version of node, what operating system, how much CPU cores, can all be referenced when talking about a runtime environment.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 12. ***What is a stub?*** 
Stubbing and verification for node.js tests. Enables you to validate and override behaviour of nested pieces of code such as methods, require() and npm modules or even instances of classes. This library is inspired on node-gently, MockJS and mock-require.  

**Features of Stub:**  

* Produces simple, lightweight Objects capable of extending down their tree
* Compatible with Nodejs
* Easily extendable directly or through an ExtensionManager
* Comes with predefined, usable extensions

Stubs are functions/programs that simulate the behaviours of components/modules. Stubs provide canned answers to function calls made during test cases. Also, you can assert on with what these stubs were called.

A use-case can be a file read, when you do not want to read an actual file:
```javascript
var fs = require('fs');

var readFileStub = sinon.stub(fs, 'readFile', function (path, cb) {  
  return cb(null, 'filecontent');
});

expect(readFileStub).to.be.called;  
readFileStub.restore();
```

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 13. ***What is a test pyramid?***
The "Test Pyramid" is a metaphor that tells us to group software tests into buckets of different granularity. It also gives an idea of how many tests we should have in each of these groups. It shows which kinds of tests you should be looking for in the different levels of the pyramid and gives practical examples on how these can be implemented.

![alt text](https://github.com/learning-zone/nodejs-interview-questions/blob/master/assets/testPyramid.png "Test Pyramid")


Mike Cohn's original test pyramid consists of three layers that your test suite should consist of (bottom to top):

1. Unit Tests
1. Service Tests
1. User Interface Tests

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 14. ***How can you secure your HTTP cookies against XSS attacks?***

1. When the web server sets cookies, it can provide some additional attributes to make sure the cookies won't be accessible by using malicious JavaScript. One such attribute is HttpOnly.

Example :
```javascript
Set-Cookie: [name]=[value]; HttpOnly
```

HttpOnly makes sure the cookies will be submitted only to the domain they originated from.

2. The "Secure" attribute can make sure the cookies are sent over secured channel only.

Example :
```javascript
Set-Cookie: [name]=[value]; Secure
```
3. The web server can use X-XSS-Protection response header to make sure pages do not load when they detect reflected cross-site scripting (XSS) attacks.

Example :
```javascript
X-XSS-Protection: 1; mode=block
```
4. The web server can use HTTP Content-Security-Policy response header to control what resources a user agent is allowed to load for a certain page. It can help to prevent various types of attacks like Cross Site Scripting (XSS) and data injection attacks.

Example :
```javascript
Content-Security-Policy: default-src 'self' *.http://sometrustedwebsite.com
```

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 15. ***How can you make sure your dependencies are safe?***
The only option is to automate the update / security audit of your dependencies. For that there are free and paid options:

1. npm outdated
2. Trace by RisingStack
3. NSP
4. GreenKeeper
5. Snyk
6. npm audit
7. npm audit fix

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 16. ***What is Event loop in Node.js?***
The event loop is what allows Node.js to perform non-blocking I/O operations — despite the fact that JavaScript is single-threaded — by offloading operations to the system kernel whenever possible.

Node.js is a single-threaded application, but it can support concurrency via the concept of `event` and `callbacks`. Every API of Node.js is asynchronous and being single-threaded, they use `async function calls` to maintain concurrency. Node uses observer pattern. Node thread keeps an event loop and whenever a task gets completed, it fires the corresponding event which signals the event-listener function to execute.

**Event-Driven Programming**  
In an event-driven application, there is generally a main loop that listens for events, and then triggers a callback function when one of those events is detected.

Although events look quite similar to callbacks, the difference lies in the fact that callback functions are called when an asynchronous function returns its result, whereas event handling works on the observer pattern. The functions that listen to events act as Observers. Whenever an event gets fired, its listener function starts executing. Node.js has multiple in-built events available through events module and EventEmitter class which are used to bind events and event-listeners as follows
```javascript
// Import events module
var events = require('events');

// Create an eventEmitter object
var eventEmitter = new events.EventEmitter();
```
Example
```javascript
// Import events module
var events = require('events');

// Create an eventEmitter object
var eventEmitter = new events.EventEmitter();

// Create an event handler as follows
var connectHandler = function connected() {
   console.log('connection succesful.');
  
   // Fire the data_received event 
   eventEmitter.emit('data_received');
}

// Bind the connection event with the handler
eventEmitter.on('connection', connectHandler);
 
// Bind the data_received event with the anonymous function
eventEmitter.on('data_received', function() {
   console.log('data received succesfully.');
});

// Fire the connection event 
eventEmitter.emit('connection');

console.log("Program Ended.");
```

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 17. ***What is REPL?***
REPL (READ, EVAL, PRINT, LOOP) is a computer environment similar to Shell (Unix/Linux) and command prompt. Node comes with the REPL environment when it is installed. System interacts with the user through outputs of commands/expressions used. It is useful in writing and debugging the codes. The work of REPL can be understood from its full form:

* **Read**: It reads the inputs from users and parses it into JavaScript data structure. It is then stored to memory.
* **Eval**: The parsed JavaScript data structure is evaluated for the results.
* **Print**: The result is printed after the evaluation.
* **Loop**: Loops the input command. To come out of NODE REPL, press ctrl+c twice

Simple Expression
```javascript
$ node
> 10 + 20
30
> 10 + ( 20 * 30 ) - 40
570
>
```

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 18. ***What is the difference between Asynchronous and Non-blocking?***
* **Asynchronous**: The architecture of asynchronous explains that the message sent will not give the reply on immediate basis just like we send the mail but do not get the reply on an immediate basis. It does not have any dependency or order. Hence improving the system efficiency and performance. The server stores the information and when the action is done it will be notified.

* **Non-Blocking**: Nonblocking immediately responses with whatever data available. Moreover, it does not block any execution and keeps on running as per the requests. If an answer could not be retrieved than in those cases API returns immediately with an error. Nonblocking is mostly used with I/O(input/output). Node.js is itself based on nonblocking I/O model. There are few ways of communication that a nonblocking I/O has completed. The callback function is to be called when the operation is completed. Nonblocking call uses the help of javascript which provides a callback function.

* **Asynchronous VS Non-Blocking**  

1) Asynchronous does not respond immediately, While Nonblocking responds immediately if the data is available and if not that simply returns an error.
2) Asynchronous improves the efficiency by doing the task fast as the response might come later, meanwhile, can do complete other tasks. Nonblocking does not block any execution and if the data is available it retrieves the information quickly.
3) Asynchronous is the opposite of synchronous while nonblocking I/O is the opposite of blocking. They both are fairly similar but they are also different as asynchronous is used with a broader range of operations while nonblocking is mostly used with I/O.

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 19. ***How to debug an application in Node.js?***
* **node-inspector**
```
npm install -g node-inspector
```
Run
```
node-debug app.js
```

* **Debugging**
    * Debugger
    * Node Inspector
    * Visual Studio Code
    * Cloud9
    * Brackets

* **Profiling**
```
1. node --prof ./app.js
2. node --prof-process ./the-generated-log-file
```
* **Heapdumps**
    * node-heapdump with Chrome Developer Tools

* **Tracing**
    * Interactive Stack Traces with TraceGL

* **Logging**  
Libraries that output debugging information
    * Caterpillar
    * Tracer
    * scribbles

Libraries that enhance stack trace information  
* Longjohn

<div align="right">
    <b><a href="#">back to top</a></b>
</div>

#### 20. ***What are some of the most popular modules of Node.js?***
* **Async**: Async is a utility module which provides straight-forward, powerful functions for working with asynchronous JavaScript.
* **Browserify**: Browserify will recursively analyze all the require() calls in your app in order to build a bundle you can serve up to the browser in a single <script> tag.
* **Bower**: Bower is a package manager for the web. It works by fetching and installing packages from all over, taking care of hunting, finding, downloading, and saving the stuff you’re looking for.
* **Backbone**: Backbone.js gives structure to web applications by providing models with key-value binding and custom events, collections with a rich API of enumerable functions, views with declarative event handling, and connects it all to your existing API over a RESTful JSON interface.
* **Csv**: csv module has four sub modules which provides CSV generation, parsing, transformation and serialization for Node.js.
* **Debug**: Debug is a tiny node.js debugging utility modelled after node core's debugging technique.
* **Express**: Express is a fast, un-opinionated, minimalist web framework. It provides small, robust tooling for HTTP servers, making it a great solution for single page applications, web sites, hybrids, or public HTTP APIs.
* **Forever**: A simple CLI tool for ensuring that a given node script runs continuously (i.e. forever).
* **Grunt**: is a JavaScript Task Runner that facilitates creating new projects and makes performing repetitive but necessary tasks such as linting, unit testing, concatenating and minifying files (among other things) trivial.
* **Gulp**: is a streaming build system that helps you automate painful or time-consuming tasks in your development workflow.
* **Hapi**: is a streaming build system that helps you automate painful or time-consuming tasks in your development workflow.
* **Http-server**: is a simple, zero-configuration command-line http server. It is powerful enough for production usage, but it's simple and hackable enough to be used for testing, local development, and learning.
* **Inquirer**: A collection of common interactive command line user interfaces.
* **Jquery**: jQuery is a fast, small, and feature-rich JavaScript library.
* **Jshint**: Static analysis tool to detect errors and potential problems in JavaScript code and to enforce your team's coding conventions.
* **Koa**: Koa is web app framework. It is an expressive HTTP middleware for node.js to make web applications and APIs more enjoyable to write.
* **Lodash**: The lodash library exported as a node module. Lodash is a modern JavaScript utility library delivering modularity, performance, & extras.
* **Less**: The less library exported as a node module.
* **Moment**: A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
* **Mongoose**: It is a MongoDB object modeling tool designed to work in an asynchronous environment.
* **MongoDB**: The official MongoDB driver for Node.js. It provides a high-level API on top of mongodb-core that is meant for end users.
* **Npm**: is package manager for javascript.
* **Nodemon**: It is a simple monitor script for use during development of a node.js app, It will watch the files in the directory in which nodemon was started, and if any files change, nodemon will automatically restart your node application.
* **Nodemailer**: This module enables e-mail sending from a Node.js applications.
* **Optimist**: is a node.js library for option parsing with an argv hash.
* **Phantomjs**: An NPM installer for PhantomJS, headless webkit with JS API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.
* **Passport**: A simple, unobtrusive authentication middleware for Node.js. Passport uses the strategies to authenticate requests. Strategies can range from verifying username and password credentials or authentication using OAuth or OpenID.
* **Q**: Q is a library for promises. A promise is an object that represents the return value or the thrown exception that the function may eventually provide.
* **Request**: Request is Simplified HTTP request client make it possible to make http calls. It supports HTTPS and follows redirects by default.
* **Socket.io**: Its a node.js realtime framework server.
* **Sails**: Sails : API-driven framework for building realtime apps, using MVC conventions (based on Express and Socket.io)
* **Through**: It enables simplified stream construction. It is easy way to create a stream that is both readable and writable.
* **Underscore**: Underscore.js is a utility-belt library for JavaScript that provides support for the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
* **Validator**: A nodejs module for a library of string validators and sanitizers.
* **Winston**: A multi-transport async logging library for Node.js
* **Ws**: A simple to use, blazing fast and thoroughly tested websocket client, server and console for node.js
* **Xml2js**: A Simple XML to JavaScript object converter.
* **Yo**: A CLI tool for running Yeoman generators
* **Zmq**: Bindings for node.js and io.js to ZeroMQ .It is a high-performance asynchronous messaging library, aimed at use in distributed or concurrent applications.
	
<div align="right">
    <b><a href="#">back to top</a></b>
</div>
