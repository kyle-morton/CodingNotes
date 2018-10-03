# NodeJS Notes 

https://app.pluralsight.com/library/courses/nodejs-getting-started

---------------------


## Concept 1
-Node allows you to run javascript server-side by providing a wrapper around the V8 JavaScript Runtime (same used in chrome, firefox, etc)

## Concept 2
-Asynchronous APIs w/out ever needing to deal w/ threads.
--allows multiple requests to come in w/out freezing your API w/ a lack of threads since Node is single-threaded.

## Concept 3
-Whenever a script is completed, the "Event Loop" in NodeJS is started. 
-This cycles until it gets an event (file done reading, etc), then it emits the necessary event which triggers any set callbacks.

__KEY:__ Asynchronicity from events which are monitored by the event loop. *Nothing blocks, all asynchronous!*

## Running a script file - Module/Exports

//node wraps your code in the below function -> this is why you can use exports, module, require right out of the box!!
````
//function (exports, module, require, __filename, __dirname) {

let g = 1; //local variable inside the LOCAL wrapping function, NOT GLOBAL

console.log(arguments);

//both are valid (exports is just a ref to module.exports)
exports.a = 42; 
module.exports.b = 41; 

// return module.exports; //implied return for the wrapping function (why you can do exports in your file)

//}

````

__Module.exports__ is just your local API that you're setting up. You can add to it (or even reassign) it if needed


## Global

-stores global variables for use in any module. 
-bad practice to store variables here!



##__Event Loop__
-what node uses to process async actions and interface them for you (so you don't have to deal w/ threads).

ex. timer -> Event Loop will monitor the timer and call your callback when it's needed

__On unhandled exception -> Event Loop will stop and app will crash__


## Node Clusters
-monitors nodeJS processes for crash
-how prod will continue to even if one node process errors out (1 for each CPU core)
-will switch to another process on error (or restart single process if single core)

## 'Error-First' callback pattern
-Node normally uses the callback parameter convention of (error, data1, data2, ...)

```
fs.readfile(__filename, function(err, data) {
    console.log("this is error first callback!');
});
```

### 'Pyramid of Doom' callback-nesting & Node's Async Pattern
-instead of nesting callbacks (which are hard to read/maintain), use the promise pattern of promise().then() etc.
-you can convert existing callbacks into promises using the util.promisify function. 

```
 
 //wrap function in promises
 const readfile = util.promisify(fs._readfile);

//creating an async function to await readFile promise
async function main() {
    const data = await readFile(__filename);
    console.log('file data is ', data);

}

main();

```

-promises are the standard as opposed to callbacks.

### Event Emmitters

```
const EventEmitter = require('events');

const myEmitter = new EventEmitter();

myEmitter.on('TEST', () => {
    console.log('event emitted!');
});

myEmmiter.emit('TEST');

```

-with event loop to delay execution

```
const EventEmitter = require('events');

const myEmitter = new EventEmitter();

//this places the callback on the event-loop so it gets called on every tick of the loop (so after all other lines have finished)
setImmediate(() => {
    myEmmiter.emit('TEST');
})

myEmitter.on('TEST', () => {
    console.log('event emitted!');
});


```

###Simple Web Server

```

const http = require('http');

//create server and bind to incoming request event w/ custom event-handler
http.createServer((req, res) => {
 
  //request & response are streams - req is readable, res is writable

  console.dir(req, { depth: 0});
  res.end('Hello World\n');
});

//start listening of server for given port
server.listen(4242, () => {
  console.log('Your NodeJS Server is running...');
});

```

__NOTE:__ Run the below code to quickly get a package.json w/ defaults

```
npm init --yes
```

###Web Server Frameworks

####Express
-works differently as each url/operation has it's own request handler
-makes working w/ web server in nodeJS __much easier__

```

//express top-level api returns a function
const express = require('express');

const server = express(); //call it here to get new server

//url handlers come BEFORE server.listen
server.get('/', (req, res) => {
  res.send({ Name: 'Kyle' });
});

server.get('/about', (req, res) => {
  res.send('about page!');
});

server.listen(4242, () => {
  console.log('Express Server is running...');
});

```

### Template languages
-works as view engine for your server. Serve pages from these frameworks to allow dynamic, server-side data

-Pug (formerly Jade)
-Handlerbars
-EJS
-JSX


### OS Module (os)
-use for interfacing w/ the host operating system that node is running on

### FS Module (fs)
-large built-in module for interfacing w/ files in NodeJS

### Child Process (child_process)
-allows you to run any os processes in a command shell inside of your Node app


### Debugging Node apps
-use below command on a given node file to debug using chrome tools

```

node --inspect-brk name_of_file.js

```

-once run, go to chrome then chrome://inspect in the url.
-find your process within list of remote targets that you can debug, then click inspect

__Note__: this works w/ javascript only.

-------

## File Uploads 

### Read from file sent thru request

```
var server = http.createServer();
server.on('request', function(request, response) {
  
  response.writeHead(200);


  /* WITHOUT REQUEST.PIPE */

  //read each chunk of file til complete
  request.on('readable', function() {
    var chunk = null;
    while ((chunk = request.read()) !== null) {
      console.log(chunk.toString()); //toString the chunk (b/c it's a buffer)
      response.write(chunk); //toString not necessary b/c response.write does it for us
    }
  });

  //send response on request end
  request.on('end', function(){
    response.end();
  });

  /* WITH REQUEST.PIPE */

  request.pipe(response); //pipes all request data back to response
  
});

```

### Read file from request, write to file

```

var server = http.createServer();
server.on('request', function(request, response) {
  
  var newFile = fs.createWriteStream('file_copy.md');
  request.pipe(newFile); //direct request content stream to new file

  request.on('end', function() {
    console.log('uploaded!');
  });

});

```

### File Upload Progress

```

var server = http.createServer();
server.on('request', function(request, response) {
  
  var newFile = fs.createWriteStream('file_copy.md');
  var fileBytes = request.headers['content-length']; //get file length

  var uploadedBytes = 0; //counter
  
  //readable event is just to keep track of current progress
  request.on('readable', function() {
    var chunk = null;
    while(null !== (chunk = request.read())) {
      
      uploadedBytes += chunk.length;
      var progress = (uploadedBytes / fileBytes) * 100; //current over total * 100

      response.write('progress: ' + parseInt(progress, 10));

    }
  })

  //direct request content stream to new file
  request.pipe(newFile); 

});

```
