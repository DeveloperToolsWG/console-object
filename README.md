Introduction
=======
A console(logging) API is available in every "modern" ECMAScript runtime, though it has never been formalized into specification.  Commonality in APIs has traditionally happened through innovation by one vendor and copy of things which are useful and adopted by another.

Historically, it has been up to developers to learn variants and ensure that they code against only the universally supported subset and learn any quirks/gaps that might affect their ability to run anywhere.  

Goals
=====
**Codify the de-facto standards for logging.**  
As a standard module, _not a part of the language itself_.  

**Provide a basis/process for new proposals.** 
Instead of pure innovate/copy, create a spec against which new features can be proposed and prollyfilled/polyfilled independently of significant language revisions.

**Promote code portability.**  

Non-goals
========
**Propose new features which are not already part of a de-facto standard.** 
Some features are widely implemented, but not universally.

**Specify too deeply what the logging system must do with input.**
Features should be described in universal logging API terms.  In the past some browsers have assumed the console object was only present if there was something to log to (i.e. a developer console was open), that creates problems that can go uncaught until delivery and is unfortunate.  Some APIs specify that error messages "will be red" or "have an (x) icon" in the console, those sorts of things should be left to the console implementations themselves.


Proposal
=======
A new ECMAScript logging module is defined (and imported by the Standard Prelude).  It contains the following methods/signatures:



```
// sends a message of the appropriate level to the logging system
// supported levels are 'debug', 'info', 'warn', 'error' (log is an alias for debug)
// message can contain standard % tokens and optional args provide data
console[level](message, ...);


// send a message containing the properties of an object to the logging system (debug level)
console.dir(obj);

// Sends an error level message to the logging system in the event that the expression evaluates to false
console.assert(expression, [message]);
```

Examples:

```
 // sends a (debug level) message of 'foo' to the logging system
console.log("foo"); 

 // sends a (debug level) message of 'hello brian, how are you?' to the logging system
console.log("hello %s, how are you?", "brian");

// sends an error level message of 'Uh oh' to the logging system
console.assert(a + b, "Uh oh!");
```
