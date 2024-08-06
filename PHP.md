PHP is a general-purpose and [[Object-Oriented-Paradigm]] language designed to be used on the server. 

For arch just install the php package.

In order to create a local server for your web directory you can use the code snippet below:

```Shell
php -S localhost:<port> -t <path-to-directory>
```

*The port can be any unoccupied port on the client computer.*

# Syntax

In order to write php code, you have to write them within a \<?php tag
```php
<?php


?>
```

You don't need to close the php tag if you don't have any html tags in the file

```php
<body>
	<h3><?php echo "Hello, World!"; ?></h3>
</body>
```

variables start with the dollar sign.
```php
$variable = "hello";
```
php is also dynamically typed for the most part. A variable can cotain any type of data.

To print type, size and the value of a variable, use the var_dump() function.

```php
var_dump($variable); //string(5) "hello"
```

Concatenation is done by the . operator

```php
$str1 = "hello";
$str2 = "world";
$concat = $str1 . $str2;
```

**You can format variables into strings when using double quotations.**


You can make key value pairs

```php
$dict = array(
	"Key" => "Value",
	"Value" => "pair",
);
```

In global space, you can define constants in two ways

```php
define('My_WEBSITE_URL', "https://amaweb.ir");
const MY_WEBSITE_URL = "https://amaweb.ir";
```

define can not be used while defining varaibles in classes.

These are the control structure syntax.

```php
do { // do-while

} while (conditon);

while()// while
{
}

for ($i=0; $i<10; $i++)// for
{
}

foreach ($list as $element)// foreach

if (condition) { // if-else constuct
} else if (condition) {

}
else {

}

switch($i) // switch-case construct
{
case 1:
	break;
case 2:
	break;
...
default:
	echo "something";
}
```

You can define functions, anonymous functions and arrow functions

```php
function foo(int(optional) $a): int(optional) {
return 0;
}
function () {

};

fn() => 5;

function sum(...$numbers)// using rest operator
{

}

function sum($a, $b) {   |  sum(...$list)//seperate a list into parameters    
					     |
}
```

**Variable scope works the same as c++.**

You can however define a global variable inside a local space such as functions.

```php
$a = 6;
function() {
	global $a;
	$a = 5;// you can't both initialize and globalize a variable on the same line
	echo $a // 5
}
```

**You can't use global variables in anonymous functions**

**put the use() keyword in front of an anonomys function declation to use a global variable.**

```php
$c = 6;
$sum = function($a, $b) use($c) {
	return $a + $b + $c;
}
```

**You can use global variables in arrow functions without use() but you can't change your global variables**

# Superglobals

It's an array that contains all the global variables a php session has access to.

You have access to all global variables on the php session

```php
var_dump($GLOBALS)
```

$\_GET contains the global variables that was given to a php page while loading. You can give a php page variables and parameters on the URL like this:

```php
localhost:8000/login.php?username=amasirat&password=asas

var_dump($_GET) =====> array(2) username => amasirat, password => asas
```

You should not manually send webpages values like this. You can do this through a form in [[HTML]] by doing an action with the "get"  method.

```php
<html>
<body>
// you can optionally omit the explicit method attribute
	<form action="/login.php" method="get">
		<input type="text" name="username">// username will be the given key
		<input type="password" name="password">// password will be the given key
		<input type="sumbit" value="Send">
	</form>
</body>
</html>
```

Data like this is usually sent using the post method. Instead of get, put post inside the method attribute of form. Post variables are found in the $\_POST array.

$\_FILES contains any files that are sent to a webpage. 

Once a file is uploaded, that file will remain inside a temperory location. To move it to a permanent location you can use the move_uploaded_file() function. Enter the temperory name of the image using $\_FILES\["image"]\["tmp_name] and the intended permanent path. (The name it has to be saved with has to be included in the path).

$\_SERVER contains server info.

$\_REQUEST contains all GET or POST or COOKIE data.

**A POST data with the same name as a GET data is printed instead.**

# Cookies and Sessions


**Cookies are data that are saved on the user's browser.**

**Sessions are data that are saved on the server.**

To set a cookie for the current webpage use the setcookie() function.

```php
setcookie(
	"site_name",//name of the cookie
	"Amaweb",//value of the cookie
	[
		'expires' => time() + 60//when the cookie expires
		'domain' => 'localhost'//the domain of the cookie, use . to appply to all subdomains.
		'path' => '/'
		'httponly' => 'true'// httponly means the cookie can only be accessed by the server and not in a client-side means like using javascript.
	],
);
```


![[2024-08-06_10-56.png]]

$\_COOKIE is superglobal array containing all cookies of the current page.

**By putting the expires key to a time in the past, it will delete the cookie**.

```php
//__DIR__ is the current script's directory.
session_start([
	'name' => 'amaweb',
	'save_path' => __DIR__ . "/sessions",
]);

$_SESSION["auth"] = true;

unset($_SESSION["auth"]);// delete the session

session_destroy();//destorys all session variables
```

**Some security concerns:**

You should never save sessions inside the public directory of your website.




