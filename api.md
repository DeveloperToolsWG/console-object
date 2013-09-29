Implementations should use a proxy implementation to ensure that calling unimplemented methods will never throw.


-------------------
## Methods
 
#### `console.assert(expression, object)` 

If the specified expression is false, the message is written to the console along with a stack trace. In the following example, the assert message is written to the console only when the document contains fewer than five child nodes:
```javascript
var list = document.querySelector('#myList');
console.assert(list.childNodes.length < 10, "List item count is > 10");
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
function login(user) {
    console.count("Login called");
    // login() code...
} 
```

#### `console.debug(object [,object, ...])` 

This method is an alias for to `console.log()`.


#### `console.dir(object)` 

Prints a JavaScript representation of the specified object. If the object being logged is an HTML element, then the properties of its DOM representation are displayed, as shown below:
```javascript
console.dir(document.body);
```

You can also use the object formatter (%O) in a `console.log()` statement to print out an element's JavaScript properties:

```javascript
console.log("document body: %O", document.body);
```

Calling `console.dir()` on a JavaScript object is equivalent to calling `console.log()` on the same object—they both print out the object's JavaScript properites in a tree format.


#### `console.dirxml(object)` 

Prints an XML representation of the specified object, as it would appear in the Elements panel. For HTML elements, calling this method is equivalent to calling `console.log()`.

```javascript
var list = document.querySelector("#myList");
console.dirxml();
```

%O is a shortcut for dir %o acts either as dir or dirxml depending on the object type (non-dom or dom)

#### `console.error(object [, object, ...])` 

Similar to `console.log()`, `console.error()` and also includes a stack trace from where the method was called.

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

Prints an error message and stack trace of JavaScript execution (This exists in firebug, I think it is essentially `console.error`).



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
console.log("A group-less log trace.");
```


#### `console.groupEnd()` 

Closes the most recently created logging group that previously created with `console.group()` or `console.groupCollapsed()`. See `console.group()` and `console.groupCollapsed()` for examples.


#### `console.info(object [, object, ...])` 

This method is identical to `console.log()`


#### `console.isIndependentlyComposed(object)` 

Todo... describe this.


#### `console.log(object [, object, ...])` 

Displays a message in the console. You pass one or more objects to this method, each of which are evaluated and concatenated into a space-delimited string. The first parameter you pass to `console.log()` may contain format specifiers, a string token composed of the percent sign (%) followed by a letter that indicates the formatting to be applied.


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


#### `console.table(data[, columns])` 

Allows to log provided data using tabular layout.  `data` can be an array of arrays or list of objects), the optional second (array) parameter can be used to filter specific particular columns/properties to be logged.


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
* Safari and Chrome used to support `console.markTimeline()` but this was deprecated after discussion with Firebug and `timeStamp()` was agreed on. (*webkit Ticket link?*)


#### `console.trace()` 

Prints a stack trace from the point where the method was called, including links to the specific lines in the JavaScript source. A counter indicates the number of times that `console.trace()` method was invoked at that point.

#### `console.warn(object [, object, ...])` 

This method is like `console.log()` but also displays a yellow warning icon along with the logged message.
```javascript
console.warn("User limit reached! (%d)", userPoints);
```

### Format Specifiers
Format specifiers are supported by some `console` methods, they allow developers to suggest specifically formatted data be output.

| Specifier         | Description                                                   |
|:----------------- |:--------------------------------------------------------------| 
| `%s`              | Formats the value as a string                                 |
| `%d`              | Formats the value as an integer                               |
| `%f`              | Formats the value as a floating point value                   |
| `%o`              | Formats the value as an expandable DOM Element                |
| `%O`              | Formats the value as an expandable JavaScript Object          |
| `%c`              | Formats the output string according to CSS styles you provide |

* Firebug support limiting the number of decimal places via `2f`.


------------
## Properties

#### `console.profiles` 




