
Javascript is a language first created for the browser, however later able to be used browser-agnostically using node.js.

A great learning resource is [The Modern Javascript Tutorial](https://javascript.info/)

# General

* The program's in javascript are called scripts.

* These scripts can run by any device that has a [Javascript engine](https://en.wikipedia.org/wiki/JavaScript_engine)

* Most browsers have an embedded engine called a JavaScript virtual machine. Chrome, Opera, and Edge have [V8], while Firefox has [SpiderMonkey].

**JavaScript can remember data on the client-side**

In order to protect safety of users, there are things that Javascript can't do. 

* It has no direct access to operating system, so no read/write on hard disk.

* Different tabs generally do not know about each other unless specifically coded to know and both pages agree for data exchange. This is called the [Same Origin Policy]

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

