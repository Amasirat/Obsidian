
Laravel is a [[PHP]] is the defacto number 1 web development framework.

## Install

Use the composer package manager on linux to use Laravel

```shell
$ composer create-project laravel/laravel example
```

Then use artisan to spin up a local server

```Shell
$ php artisan serve
```

## General Info

The framework uses an [[MVC]] design pattern to organize application development.

Views are located at resources/views. The resources directory also holds CSS and JS scripts.

The routes directory holds web.php file which contains functions that handle routing. They get a route and return what that route needs to send to the browser. Common practice is to return your views.

Like any other web development frameworks, Laravel allows you to create reusable components. Simply create a "components" inside "resources/views/" and place your components there. 

You can assign variables to aspects of the component. That way any file that extends from them can choose to assign their own value to it. forexample, using a $title variable for each view to have its own title. 

**$slot is a default variable used to signify "This is where any file that extends the component will place its own unique information."

These variables are generally called slot variables (which slot information in place), only usuable through the [[Blade Template Engine]].

**$attributes also outputs any attributes given to the conmponent tag.**

This is an example layout component:

```HTML
<!DOCTYPE html>
<html>
	<head>
		<title>{{ $title }}</title>
	</head>
	<body>
		{{ $slot }}
	</body>

</html>
```

Then while creating any other html file, you can use this syntax to wrap its information around it.

```HTML
<x-layout>
<!-- put x-(the name of the component) to refer to an existing component. -->
	<x-slot:title>Contact</x-slot:title>
	<h1>Hi, this is contacts page</h1>
</x-layout>
```

In Blade components there are **attributes** and **props**. Attributes are just regular html attributes.
props are custom made attributes that are not regular html tags however they are used like one inside a view.

```PHP
@props(["active" => false])//an associative array of props
```

```HTML
<x-nav-link href="whatever" :active="false"></x-nav-link>
```

Inside blade files, prefixing a prop or attribute with ':' tells it to treat the value inside as an expression rather than a string.

## Eloquent

Eloquent is an ORM [[Object Relational Mapper]]. It maps an object in the database to an object in PHP. It basically represents database objects as PHP code.

Simply extend a definition of a class in PHP with Model.

Eloquent convention dictates that a database and object have to have similar naming schemes. And the object in PHP has to be the single form of the table name in database.

If the database table has a different naming convention to your class name in PHP, you can define a protected member variable which includes the name of the required table.

```PHP
use Illuminate\Database\Eloquent\Model;
class Job extends Model {
	protected $table = "job-listings";
}
```

You can create models using the artisan script. 

```Shell
$ php artisan make:model Flight -m \\-m means create migration as well
```

It will create a class inside App\\Models namespace, along with a migration file.

**Eloquent does not allow mass assignments for security reasons, you can create a $fillable variable containing arrays of fields that a user is allowed to fill**

Factories allow you to automatically populate a database table with fake data for testing purposes. You can create a factory for a specific model by including the flag -f while making the model through artisan.