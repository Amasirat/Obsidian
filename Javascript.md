
Javascript is a language first created for the browser, however later able to be used browser-agnostically using node.js.

A great learning resource is [The Modern Javascript Tutorial](https://javascript.info/)

# General

* The programs in javascript are called scripts.

* These scripts can run by any device that has a [Javascript engine](https://en.wikipedia.org/wiki/JavaScript_engine)

* Most browsers have an embedded engine called a JavaScript virtual machine. Chrome, Opera, and Edge have [V8], while Firefox has [SpiderMonkey].

**JavaScript can remember data on the client-side**

In order to protect safety of users, there are things that Javascript can't do. 

* It has no direct access to operating system, so no read/write on hard disk.

* Different tabs generally do not know about each other unless specifically coded to know and both pages agree for data exchange. This is called the [Same Origin Policy]

Since 2009, there is such a thing as modern Javascript that is different from the old way, Having the "use strict" before javascript code, the engine will assume modern javascript functionality.

```Javascript
"use strict"
```

# Usage and Syntax

You can use Javascript inside html files by putting Javascript code inside a \<script\> tag

```html
<script>
	alert( 'Hello World!' );
</script>
```

You can also link to JS scripts for more complicated and longer javascript scripts.

```html
<script src="/path/to/JS"></script>
```

*Note: if you put src inside a \<script\> tag, you won't be able to put code inside it anymore.

More complex javascript code will be put inside a seperate file because the browser will download it and store in its cache, so it'll be a lot more efficient.

We use let to declare variables:

```Javascript
let myvar = 5;
```

const for unchanging variables:

```Javascript
const BIRTHDAY = "27/8/2004";
```

var is the old method of declaring variables which has subtle differences to let:

```Javascript
var myvar = 8;
```

common practice is to name literal constants in uppercase and variables that are calculated at runtime like any normal variable:

```Javascript
const COLOR_WHITE = "#00000";

let age = calculateAge(currentYear);
```

*Pascal case is common in javascript ecosystem as well.*

# Datatypes

There are 8 Datatypes in Javascript.

**Javascript variables can store different types of variables and they can also change the type of value they store.**

### Numbers:

Integers and floating point numbers are both known as the **number** type in javascript. There are special number values however:

* **Infinity**
```Javascript
let number = Infinity;
```
* **-Infinity**
```Javascript
let number = -Infinity;
```
* **NaN**: Stands for not a number.
Any operation that cantains NaN's result will be NaN except the power operator

```Javascript
let num = NaN ** 0 //result will be 1
```
### Bigint

For storing large numbers that exceed the range that fits inside a signed 64bit number. append 'n' at the end of a number to dedicate it as such.

```Javascript
let bigintnum = 12324845845121545451215n;
```

Doing mathematics in Javascript is generally safe. The result of divide by zero will be Infinity.

### String

Javascript doesn't have char type, only a string. The rest is much like how it is in python.

Use backticks to create strings with "extended functionality" that for example can take variables using ${}.

```Javascript
alert(`hello ${name}`);
```

### Booleans

Self-explanatory. No need to explain further.

### Objects

Objects are a special type that hold more complex data.

### Symbol

Symbol is used to create unique identifiers for objects.

### Special Values

There are some special values variables can take that do not have anything to do with the types mentioned:

* the "null" value:
To represent variables with empty values. **It is not the same as the null pointer, it is only an empty value**.
* Undefined:
When a variable is not given a value at the point of declaration, it will hold an" "undefined" value.

### Typeof operator

Returns type of an operand in the form of a string

```Javascript
let x = "name"

typeof x //will evaluate to "string"

typeof null //will evaluate to "object" which is a language bug, the null value is not an object

typeof alert //"function"
```

functions are not types in javascript, so typeof alert evaluating to "function" is technically not correct, however it's a convenient bug to use.

# Interaction functions

Modal windows in browsers are windows that the user has to deal with before being able to interact with the rest of the website.

Three common functions for these types of modal functionality are:

* alert(): simply alerts user.
* prompt("title", optional: "example"): Prompts user for info, it returns the user's answer, if user quit the prompt window, it will return null. the second parameter is largely optional.
```Javascript
let userAge = prompt("What is your age?", 18);
```
* confirm(): Asks user for confirmation, returns true if yes and false if no.


# Comparisons

In javascript, while checking for equality, variables of different types are converted:

```Javascript
"3" == 3 // True
"null" == 0 //False, string of non empty value will be converted to NaN
false == 0 // True
```

Therefore, there's another operator called the strict equality that checks data without type conversion.

```Javascript
"3" === 3 // false, different types
true == 1 // false
```

# Functions

Functions are declared like this:
```Javascript
function sayHi() {
	alert("Hi!");
}

sayHi();
```

The javascript engine when running scripts will first process all global scope functions at once, therefore once a function is declared, they can be used anywhere inside that script, even above the point of declaration. 

In modern versions of javascript, functions declared inside brackets, have scopes inside the bracket only. Functions declared in global scope behave the same way
### Function Expressions

We can create functions in the middle of an expression. Kind of like lambdas in C++

```Javascript
let sayHi = function() {
	alert("Hello!");
}
```

In javascript, functions are values, so we can deal with them like values

```Javascript
function sayHi() {
alert("Hi!");
}

alert(sayHi);//without (), this will just alert the function body and is totally valid syntax.
```

We can copy a function into another variable:

```Javascript
function sayHi() {
	...
}

let func = sayHi;

func();
sayHi(); //they do the same thing
```

Example:

```Javascript
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

function showOk() {
  alert( "You agreed." );
}

function showCancel() {
  alert( "You canceled the execution." );
}

// usage: functions showOk, showCancel are passed as arguments to ask
ask("Do you agree?", showOk, showCancel);
```

showOk and showCancel are called callback functions. Functions that we expect to be called back later.

Functions in javascript are actions, strings and numbers etc represent data while functions are values that represent actions. (which is Fing up my brain right now!!!!!!!!!!!!!!!!!!!!!!!!!!)
We can pass it to functions and call them whenever we want.

### The Differences between Function Declaration and Expression

Usually, Function declaration is preferable because they can be used anywhere inside the file, but Function Expressions can only be used after the line in which they are defined(like a regular variable)

However in cases when function declaration does not cut it, for example when a function needs to be used with terneray operator (?), function expression is required.

# Arrow Functions

```Javascript
let func = (parameter1, parameter2, ...) => expression;
```

which is equal to:

```Javascript
function func(prameter1, parameter2, ...) {
	return expression;
}
```

Arrow functions are handy for simple actions, especially for one-liners. They come in two flavors:

1. Without curly braces: `(...args) => expression` – the right side is an expression: the function evaluates it and returns the result. Parentheses can be omitted, if there’s only a single argument, e.g. `n => n*2`.
2. With curly braces: `(...args) => { body }` – brackets allow us to write multiple statements inside the function, but we need an explicit `return` to return something.