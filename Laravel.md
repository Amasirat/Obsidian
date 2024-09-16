
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
$ php artisan make:model Flight -m -f \\-m means create migration as well
```

It will create a class inside App\\Models namespace, along with a migration file.

**Eloquent does not allow mass assignments for security reasons, you can create a $fillable variable containing arrays of fields that a user is allowed to fill**

Factories allow you to automatically populate a database table with fake data for testing purposes. You can create a factory for a specific model by including the flag -f while making the model through artisan.
## Model Relationships

A database can have a foreign key that contains a reference to another table's rows. That is the bedrock of [[Database Theory]]

we can also establish these relationships between different models Inside eloquent.

For example, for a Job model. A Job model belongs to an Employer model, and an Employer model has many jobs. You can simply define functions that return the type of relationship they have inside the model.

```php
class Job extends Model {

	public function employer(): BelongsTo {
		return $this->belongsTo(Employer::class);
	}
}

class Employer extends Model {
	public function job(): HasMany {
		return $this->hasMany(Job::class);
	}
}
```

This way, we can simply reference a relation of a model similar to a property and return an object of that model instead of a foreign key which it is in reality. (for example employer_id is stored inside the job-listings table in our sql database)

There are many relationship types, [More info here](https://laravel.com/docs/11.x/eloquent-relationships).
# Factories

Factories exist to create an automatic way of populating your tables for testing purposes.

```php
public function definition(): array

{
	return [
		'title' => fake()->jobTitle(),
		'employer_id' => Employer::factory(),
		'salary' => fake()->numberBetween(10, 2000),
	];
}
```

Reference another model's factory to create foreign data that your data is connected to.
```php
Job::factory(10)->create(); //use the number to create n number of fake data, no number will just create one data
```

# Pivot Tables

There are some relationships that require the use of a pivot table in order to show their relations. A job and its tags is a prime eample. A job-listing belongs to many tags and a tag can belong to many jobs. Therefore it is best to create a pivot table that records these connections through a foreign ID.

create a constrained on your foreign IDs to cascade delete if a foreign ID is missing, so that no pivot table points to an orphan ID.

```php
$table->foriegnIDFor()->constrained()->cascadeOnDelete();
```

## Eager Loading

You should be mindful about the queries that are executed in the background. 
When you access a foreign property in Eloquent, a SQL query is executed each time. Inside a loop, those extra queries become a massive issue. This problem is famously called the [[N+1]] problem.

The solution is to load whatever rows you need ahead of time.

For example for a Job Model that has a foreign link to the Employer model. you can load it with its employers like this in php:

```php 
$jobs = Job::with('employer')->get(); //this is only when data in database is not a huge number
```

In order to disable lazy loading in your project you can call the static function preventLazyLoading of Model inside the boot member function of app/Provider/AppServiceProvider.php.

```php
function boot() {
	Model::preventLazyLoading();
}
```

## Pagination

The previous eager loading only works when the data in database is small. However for massive data, it's common practice to paginate data.

```php
Job::with('employer')->paginate(15);
```

This way there will be only 15 jobs listed per page.  In order to see the other pages, a $\_GET variable of page=n needs to be added to the URL.

```
http://localhost/jobs?page=2
```

Laravel has a builtin page view. render it inside a view like this:

```php
<div>
	{{ jobs->links() }}
</div>
```
 This is only available when using pagination. 
 
 If you are using tailwind or bootstrap, this function works right out of the box. However if you are not, you can edit its style.

Change default view of paginator by writing this code inside boot() function of AppServiceProvider

```php
paginator::defaultView('nameOfView');
```

You can also copy the views of paginator by using php artisan vendor:publish and finding the pagination tag inside of it.

For simple usecases it gets the job done just fine, however it can be costly in the long run. You can use **simplePaginate** instead in that case.

You can also use cursor based pagination(**cursorPaginate**), however in this way you can not address a specific page because the way cursor pagination works is not page based. It uses a cursor variable which contains an  encoded string to address the next set of results to fetch from laravel. It is easily the most performant way of doing this.

# Database Seeders

When you freshly migrate a database, the datat that the tables held are all droped and you'll have to re-populate every table from scratch. However using seeders you can seed data into your tables right after you've migrated.

You can make new seeders in order to make isolate seeders for each of your databases.

```shell
$ php artisan make:seeder JobSeeder
```

you can exclusively run your dedicated seeder class like this:

```shell
$ php artisan db:seed --class JobSeeder
```

