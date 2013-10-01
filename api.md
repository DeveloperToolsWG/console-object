The `console` object MUST be exposed programatically regardless of whether the `console` is visible or additional tools are installed.
Implementations MUST use a proxy implementation to ensure that calling unimplemented methods will never throw.
The Logging APIs MUST accept (and make use of for logging purposes) an arbitrary number of arguments.
Implementations SHOULD follow the follow the [Format Specifiers](#format-specifiers) recommendations 
specified in this document, but MAY decide what format to log.  Implementations MUST provide 
high resolution timestamps for the [Timing](#timing) and [Profiling](#profiling) APIs specified here. 


-------------------
## Methods
 

### Logging
#### `console.assert(expression, object)` 

If the specified expression is false, the message is written to the console along with a stack trace.
```javascript
// Assuming the element matching #myList contains 10 or more children
var list = document.querySelector('#myList');
console.assert(list.childNodes.length < 10, "#myList has 10 or more children!");

> #myList has 10 or more children!
```

#### `console.clear()` 

Clears the console.
```javascript
console.clear();
```


#### `console.count(label)` 

Writes the the number of times that count() has been invoked at the same line and with the same label.

In the following example count() is invoked each time the login() function is invoked.
```javascript
function test () {
    function login(user) {
        console.count("Login called for " + user);
    } 
    login("brian");
    login("paul");
    login("paul");
}
test();

> Login called for brian: 1
  Login called for paul: 1
  Login called for paul: 2
```

#### `console.debug(object [,object, ...])` 

This method is an alias for to [`console.log()`](#consolelogobject--object-).


#### `console.dir(object)` 

Prints a JavaScript representation of the specified object. 
If the object being logged is an HTML element, the JavaScript Object view is forced (as opposed to the Element view)

```javascript
console.dir(document.body);

// the above is equivalent to ...
// console.log("%0", document.body);
````

#### `console.dirxml(object)` 

Prints an XML/HTML Element representation of the specified object if possible or  the JavaScript Object view if it is not.

```javascript
var list = document.querySelector("#myList");
console.dirxml(list);

// The above is equivalent to ...
// console.log("%o", list);
```

#### `console.error(object [, object, ...])` 

Logs an `error` level message, including a stack trace indicating where it was called.  It's input signature matches 
`console.log()`.

```javascript
function connectToServer() {
    var errorCode = 1;
    if (errorCode) {
        console.error("Error: %s (%i)", "Server is  not responding", 500);
    }
}
connectToServer();
```


#### `console.exception(error-object[, object, ...])` 

An alias for  `console.error`.



#### `console.group(object[, object, ...])` 

Starts a new logging group with an optional title. All console output that occurs after calling this method and calling `console.groupEnd() `appears in the same visual group.

```javascript
console.group("Authenticating user '%s'", user);
console.log("User authenticated");
console.groupEnd();
```

You can also nest groups:

```javascript
// New group for authentication:
console.group("Authenticating user '%s'", user);
// later...
console.log("User authenticated", user);
// A nested group for authorization:
console.group("Authorizing user '%s'", user);
console.log("User authorized");
console.groupEnd();
console.groupEnd();
```

#### `console.groupCollapsed(object[, object, ...])` 

Creates a new logging group that is initially collapsed instead of open, as with `console.group()`.

```javascript
console.groupCollapsed("Authenticating user '%s'", user);
console.log("User authenticated");
console.groupEnd();
console.log("A group-less log message.");
```


#### `console.groupEnd()` 

Closes the most recently created logging group that previously created with `console.group()` or `console.groupCollapsed()`. See `console.group()` and `console.groupCollapsed()` for examples.


#### `console.info(object [, object, ...])` 

Logs an `info` level message, it's signature is identical to [`console.log()`](#consolelogobject--object-).


#### `console.isIndependentlyComposed(object)` 

Todo... describe this.


#### `console.log(object [, object, ...])` 

Logs a `debug` level message. You pass one or more objects to this method, each of which are evaluated and concatenated into a space-delimited string. The first parameter you pass to [`console.log()`](#consolelogobject--object-) may contain [Format Specifiers](#format-specifiers).


#### `console.table(data[, columns])` 

Allows to log provided data using tabular layout.  `data` can be an array of arrays or list of objects), the optional second (array) parameter can be used to filter specific particular columns/properties to be logged.

#### `console.trace(object [, object, ...])` 

Equivalent to `console.log` except that it also adds stack trace from the point where the method was called, including links to the specific lines in the JavaScript source. A counter indicates the number of times that `console.trace()` method was invoked at that point.

#### `console.warn(object [, object, ...])` 

Logs a `warn` level message, it's signature is identical to [`console.log()`](#consolelogobject--object-).


```javascript
console.warn("User limit reached! (%d)", userPoints);
```

#### Format Specifiers
Format specifiers are supported by some `console` methods, they allow developers to suggest specifically formatted data be output.

| Specifier         | Description                                                                        |
|:----------------- |:-----------------------------------------------------------------------------------| 
| `%s`              | Formats the value as a string (cooercing via toString() if necessary)              |
| `%d`, `%i`        | Formats the value as an integer                                                    |
| `%f`              | Formats the value as a floating point value                                        |
| `%o`              | Formats the value as an expandable DOM Element (or JavaScript Object if it is not) |
| `%O`              | Formats the value as an expandable JavaScript Object                               |
| `%c`              | Formats the output string according to CSS styles you provide                      |

* Firebug support limiting the number of decimal places via `2f`.

```javascript
console.log("Hello %s", "Brian");
> Hello Brian

// If the number of values exceeds the number of formatters, inputs should be appended/space delimited 
console.log("I am %s and I have:", "1", 2, 3, 4);
> I am 1 and I have: 2 3 4

// Only the first argument to methods fill apply format specfiers.
console.log("I am", "Paul", "and I have %d:", "1", 2, 3, 4);
> I am Paul and I have %d: 1 2 3 4
```


### Profiling

#### `console.profile([label])` 

Calling this function initiates a JavaScript CPU profile with an optional label.  To complete the profile, call `console.profileEnd()`. 

In the following example a CPU profile is started at the entry to a function that is suspected to consume excessive CPU resources, and ended when the function exits.
```javascript
function processPixels() {
  console.profile("Processing pixels");
  // later, after processing pixels
  console.profileEnd();
}
```

* `console.profiles` used to be an array storing profile data. It was removed in Chrome and Safari. 
* multiple profile() calls can overlap in Chrome, however in IE11, only one recording at a time is allowed.

#### `console.profileEnd()` 

Stops the current JavaScript CPU profiling session, if one is in progress, and prints the report to the Profiles panel.
```javascript
console.profileEnd()
```



### Timing

#### `console.time(label)` 

Starts a new timer with an associated label. When `console.timeEnd()` is called with the same label, the timer is stopped the elapsed time displayed in the Console. Timer values are accurate to the sub-millisecond.
```javascript
console.time("Array initialize");
var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
    array[i] = new Object();
};
console.timeEnd("Array initialize");
```

#### `console.timeEnd(label)` 

Stops the timer with the specified label and prints the elapsed time.

For example usage, see `console.time()`.


#### `console.timeStamp([label])` 

This method adds an event to the Timeline during a recording session. This lets you visually correlate your code generated time stamp to other events, such as screen layout and paints, that are automatically added to the Timeline.

* IE11 uses `performance.mark()` from the ********** spec to mark the Timeline and does not support `console.timeStamp`
* Safari and Chrome used to support `console.markTimeline()` but this was [deprecated after discussion with Firebug](https://bugs.webkit.org/show_bug.cgi?id=63317) and `timeStamp()` was agreed on.



------------
## Properties

#### `console.profiles` 




