# Live JS With Chrome: What You Can/Can't Do #

There are specific things that you can and can't do reliably when modifying JavaScript code using Chrome's debugging features. I'll run through specific cases to describe how they work/don't work. First, a couple of high-level notes:

* It does not appear that you can modify JS that is in the HTML file. This is not tested at the API level, but it does not work within the Chrome Dev Tools UI.
* Since JavaScript is an imperative language, the only changes that can reasonably be expected to have an effect are changes to functions that will be called again.
* When doing Live JavaScript editing, the live editing icon has the disconnected appearance.
* Using TodoMVC as an example, the footer that displays the number of todo items tends to blink in a way that it does not when you're not doing live JS editing.

The following sections will describe specific kinds of cases and how they perform.

## The Setup ##

Use the `dangoor/live-js-research` branch of Brackets. All this branch does is turn on the live JS feature.

index.html:

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="jquery.min.js"></script>
        <script src="testing.js"></script>
    </head>
    <body>
        <h1>Hello</h1>
        <div onclick="logIt()">Logger</div>
        <div onclick="clearConsole()">Clear Console</div>
        <div id="console">
        </div>
    </body>
</html>
```

testing.js:

```javascript
var startTime = new Date().getTime();

function log(message) {
    var difference = parseInt((new Date().getTime() - startTime) / 1000);
    $("#console").append($("<div>" + difference + " " + message + "</div>"));
}

function clearConsole() {
    $("#console").html("");
}
```

## Simple Top-Level Functions ##

This is the simplest case. Imagine the following code attached to a click handler:

```javascript
function logIt() {
    log("global message");
}
```

You can modify that line of code, add additional lines, add code that creates local variables, access global variables. In short, functions of this sort can be modified without difficulty.

## Top-Level Function with a Closure ##

```javascript
function logIt() {
    var i = 1;
    log("global message " + i++);
    setTimeout(function () {
        log("timed message " + i);
    }, 1000);
}
```

As with the case above, this one appears to work just fine. In fact, this case was derived from the previous one without reloading, so adding a closure is fine.

## Object Construction ##

```javascript
function Thing(name) {
    this.name = name;
}

function logIt() {
    var t = new Thing("Alfred");
    setTimeout(function () {
        log("timed message " + t.name);
    }, 15);
}
```

In my file, I already had the `Thing` constructor above in the same file as the `logIt` function from the previous case. I was able to modify `logIt` into the form above without reloading.

I just also confirmed that random whitespace changes between these functions has no effect.

We now reach our first unexpected behavior. I altered the code to appear as below:

```javascript
function Thing(name, age) {
    this.name = name;
    this.age = age;
}

function logIt() {
    var t = new Thing("Alfredbot", 21);
    setTimeout(function () {
        log(t.name + " is " + t.age + " years old");
    }, 15);
}
```

When run, `logIt` displays "Alfredbot is undefined years old". If I reload the page (just by saving the JS file), this works as expected. I can alter the age provided in logIt. I also changed the `Thing` constructor to set `this.age = age * 2;`, and that also worked. I added `this.doubleAge = age * 2;` and changed `logIt` to display `t.doubleAge` instead of `t.age`, and that also worked.

I was able to change the `Thing` constructor's arguments to be `n, a` and that change also behaved just fine.

The first concrete rule:

**You cannot add a new argument to a function.**

## Adding Function Arguments, Part 2 ##

We start with our logging function:

```javascript
function log(message) {
    var difference = parseInt((new Date().getTime() - startTime) / 1000);
    $("#console").append($("<div>" + difference + " " + message + "</div>"));
}
```

You can add text to the appended log message. Make edits to get the following (add `num` argument and add `num` to the message:

```javascript
function log(message, num) {
    var difference = parseInt((new Date().getTime() - startTime) / 1000);
    $("#console").append($("<div>" + difference + " " + message + num + "</div>"));
}
```

Also modify `logIt` to pass in a num argument. I observed in making these changes that:

* Adding the argument did not work (as expected from the previous case)
* Removing the added code did not put the `log` function back into a state where live changes could be made
* Changes could still be made to `logIt`, even though `log` would no longer accept changes

## Removing an Argument ##

Save the file with the modified `log` function above. After the reload, it should log with the `num` argument just fine. Remove the `num` argument and the `+ num ` in the `log` function.

**Removing a function argument also seems problematic**

## Renaming a Function ##

Put the `log` function back to its original state. Rename the function to `logger`, but don't change the call to it from the `logIt` function. Clicking on Logger on the test page still logs, despite the fact that the name is theoretically different.

**Renaming a function has no effect**

## Adding a Function ##

Reload the page and then add a function called `doubleIt` and call that function from `logIt`. Here's the code we end up with:

```javascript
function doubleIt(a, b) {
    return (a + b) * 2;
}

function logIt() {
    var t = new Thing("Alfredbot", 22, "Sword");
    log(t.name + " is " + t.doubleAge + " years old and his weapon of choice is " + t.np + " " + doubleIt(4, 1));
}
```

Not surprisingly, `logIt` is now broken because it does not see the `doubleIt` function. This is not a surprise, because the `doubleIt` function declaration has not been executed.

**You cannot add a new function in a scope that has already been executed**

## Adding a Function by Executing Another ##

For the next cases, we'll start walking through prototype manipulation. We'll redo the setup to give us another function we can execute to work with.

Here's index.html:

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="jquery.min.js"></script>
        <script src="testing.js"></script>
    </head>
    <body>
        <h1>Hello</h1>
        <div class="resetter" onclick="resetThing()">Reset Thing</div>
        <div onclick="logIt()">Logger</div>
        <div onclick="clearConsole()">Clear Console</div>
        <div id="console">
        </div>
    </body>
</html>
```

And testing.js:

```javascript
function Thing(name, age) {
    this.name = name;
    this.age = age;
}

var startTime = new Date().getTime();

function log(message) {
    var difference = parseInt((new Date().getTime() - startTime) / 1000);
    $("#console").append($("<div>" + difference + " " + message + "</div>"));
}

function logIt() {
    var t = new Thing("Alfredbot", 22);
    log(t.name + " is " + t.age + " ");
}


function resetThing() {
    log("resetting");
}

function clearConsole() {
    $("#console").html("");
}

resetThing();
```

Be sure to reload after these changes have been made. In `resetThing`, add the `doubleIt` function to `window` (without a reload):

```javascript
function resetThing() {
    log("resetting");
    
    window.doubleIt = function(a, b) {
        return (a + b) * 2;
    }
}
```

and call `doubleIt` from `logIt`:

```javascript
    log(t.name + " is " + t.doubleAge + " years old and his weapon of choice is " + t.np + " " + doubleIt(4, 5));
 ```

 Click Reset Thing on the page, then click Logger. You'll see that doubleIt was successfully added to the page.

## Modifying a Function by Re-Running Code ##

Changing the `doubleIt` function works just fine, as you'd expect. What if we want to change the function signature? Add a `c` parameter to `doubleIt`, and also add a third number to `logIt`.

`doubleIt`'s declaration now looks like this:

```javascript
    window.doubleIt = function(a, b, c) {
        return (a + b + c) * 2;
    }
```

Click Reset Thing and then Logger. The new definition of `doubleIt` is in place.

## Adding to an Object Prototype ##

Remove `doubleIt` and the reference to it in `logIt`. Reload the page.

Add the following to `resetThing`:

```javascript
    Thing.prototype.doubleAge = function() {
        return this.age * 2;
    }
```

and call it from `logIt`:

```javascript
    log(t.name + " is " + t.age + " " + t.doubleAge());
```

Click Reset Thing, Logger. Note that it doesn't log because it can't find `t.doubleAge`. Reload and confirm that `logIt` can then find `t.doubleAge`. Once you've done this, you can change `doubleAge` and it works fine.

**Adding to an object prototype doesn't work!**

## Adding to an Object Prototype, Part 2 ##

Add this to the `resetThing` function and change `logIt` to call this instead of `doubleAge`:

```javascript
    Thing.prototype.tripleAge = function() {
        return this.age * 3;
    }
```

Click Reset/Logger. Note that adding *this* function worked where the previous one did not.

**You can add to an object prototype in a function where you previously added to it**

## Adding to a New Prototype ##

Let's see how differently it behaves when you use a different style of manipulating an object prototype:

```javascript
function Thing(name, age) {
    this.name = name;
    this.age = age;
}

var startTime = new Date().getTime();

function log(message) {
    var difference = parseInt((new Date().getTime() - startTime) / 1000);
    $("#console").append($("<div>" + difference + " " + message + "</div>"));
}

function logIt() {
    var t = new Thing("Alfredbot", 22);
    log(t.name + " is " + t.age + " ");
}


function resetThing() {
    log("resetting");
    
    Thing.prototype = {
    };
}

function clearConsole() {
    $("#console").html("");
}

resetThing();
```

Reload with that copy of testing.js. Now, add `doubleAge` in this style:

```javascript
    Thing.prototype = {
        doubleAge: function() {
            return this.age * 2;
        }
    };
```

and call it from `logIt`. Reset/Logger.

That worked for me.

## Adding to a New Prototype, Part 2 ##

Add a `tripleAge` function just as we did `doubleAge`.

```javascript
        tripleAge: function() {
            return this.age * 3;
        }
```

and call that from `logIt`. Click Reset/Logger. That works again.

# Summary of the Rules #

These are the boundaries we have seen of Chrome's live JavaScript swapping feature. The actions below do *not* work:

* Altering code that has already been executed and doesn't run again
* Adding a new argument to a function
* Removing an argument from a function
* Renaming a function
* Adding a function in a scope that has already been executed
* Adding to an object's prototype (unless you previously added to the prototype)