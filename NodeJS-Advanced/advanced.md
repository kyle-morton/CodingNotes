## Node.js Advanced topics
- pluralsight course - https://app.pluralsight.com/library/courses/nodejs-advanced/table-of-contents


### Node VMs

-Uses V8, but can use others like Chakras.

### Node CLI & REPL

-Node has it's own CLI that contains a REPL you can you to run code.

-Whenever you run a bit of code after the '>' character, it does a REPL (Read, Eval, Print, Loop) process to run the code.

```

> var arr = [];
undefined
> arr. //tab tab to autocomplete
arr.__defineGetter__      arr.__defineSetter__      arr.__lookupGetter__      arr.__lookupSetter__
arr.__proto__             arr.constructor           arr.hasOwnProperty        arr.isPrototypeOf
arr.propertyIsEnumerable  arr.toLocaleString        arr.toString              arr.valueOf

```

### Global Object

-properties on the global object are available anywhere in your code.

-avoid add properties to it.

-it already has a large list of objects bound to it (process, buffer, etc).

##### Process Object

-provides bridge between node and it's running environment.

```
 
 process.on('exit', (code) => {
     //do something when node process exits

    //NOTE: only synchronous actions allowed here (no callbacks)

 });

 process.on('uncaughtException', (err) => {
     //something went wrong
     //cleanup before exit
 })

```

##### Buffer Object

-buffer is a lower-level way of handling sequences of binary data. 

-can't be resized after initialization.

```

//build buffer of 8 elements
Buffer.alloc(8); 

//build buffer of 8 elements w/out initializing data
Buffer.allocUnsafe(8);

//allocUnsafe is taking unused spots in memory that may contain old, sensitive data so it's important to clear this
Buffer.allocUnsafe(8).fill();


```

-__Note__: any updates/slices/etc to the buffer array will modify the buffer itself and it's elements (by reference, not copy)