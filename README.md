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

**Include this work within a ECMA, W3C or WHATWG deliverable.**
For now, this work will remain outside of those groups.

**Specify the Command Line API**
As it's not used in user code, it's much lower priority to solidify.

Proposal
=======
The de-facto standard console contains the following methods/signatures:



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

// sends an error message of 'Uh oh' to the logging system (level assert?) in the event the assertion fails
console.assert(a + b, "Uh oh!");
```


### Supplementary Links

* Existing support
  * Firebug: https://getfirebug.com/wiki/index.php/Console_API
  * Chrome: https://developers.google.com/chrome-developer-tools/docs/console-api
  * IE: http://msdn.microsoft.com/en-us/library/ie/hh772169(v=vs.85).aspx
  * Firefox: https://developer.mozilla.org/en-US/docs/Web/API/console
* Previous standardization work: 
  * http://sideshowbarker.github.io/console-spec/
  * [thread on public-script-coord](http://lists.w3.org/Archives/Public/public-script-coord/2013JanMar/0152.html)
  * fitzgen (FF) said…

> what do you think about a github organization thats just docs on console apis? 

### Ideas

* `console.foo()` should never throw. ([Implemented in Firefox](https://bugzilla.mozilla.org/show_bug.cgi?id=629607))

### Notes

* I spoke with engineers behind Safari Web Inspector, Firefox Developer Tools, IE F12 Developer Tools, Chrome DevTools (circa July 2013). All are supportive of an effort like this. ~paulirish
* This document to be very communicative about what current browser support is. 
* There will be a subset of the console which MUST be implemented and a larger set of SHOULD behavior (such as the `%O` formatting of `.log()`)
* We'll also document any newly added methods, such as [`console.msIsIndependentlyComposed()`](http://msdn.microsoft.com/en-us/library/ie/hh781493\(v=vs.85\).aspx)

