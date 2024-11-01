
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

When you freshly migrate a database, the data that the tables held are all droped and you'll have to re-populate every table from scratch. However using seeders you can seed data into your tables right after you've migrated.

You can make new seeders in order to make isolate seeders for each of your databases.

```shell
$ php artisan make:seeder JobSeeder
```

you can exclusively run your dedicated seeder class like this:

```shell
$ php artisan db:seed --class JobSeeder
```

# CSRF and Forms

Imagine a local bank that has no idea what CSRF is. You have an account on there, but because CSRF is not implemented, a malicious actor can send you a fake email, telling you to change your password. Despite the email looking legit, the link is sent to a page hosted by the malicious hacker. Once you send your passwords, your data gets sent to the actual localbank, however this time the hacker can cache your new password as well. This works if you had signed in to the bank and a session cookie was stored on your browser before.

CSRF is basically a hidden token that is sent alongside the data the form sends. This token is compared to the current session token. If they don't align, the form will not get accepted. 

Laravel has support for CSRF and will not accept forms that do not have CSRF implemented. To use CSRF, just add a \@csrf somewhere inside your form. Once turned into vanila php, it will become a hidden input with a token value.

```php
<form method="POST" action="/jobs">
	@csrf
	<input type="text">
	<input type="text">
	<input type="submit" value="submit">
</form>
```

Once sent, Laravel requires a post route listening for the URL put inside the action attribute of forms.

```php
Route::post('\jobs', function() {
// do necessary validation and database manipulation
return redirect('/a get route');
});
```

For validation, you can use request()->validate(\[]) to pass in validation rules as an associative array for each of your properties. 

```php
request()->validate([
	'title' => ['required', 'min:3'],
	'salary' => ['required']
])
```

Laravel has a list of validation rules that can be used in [here](https://laravel.com/docs/11.x/validation)

In case of errors Laravel refreshes the page and returns th errors inside a $errors variable. You can use $errors->all() to view them or $errors->any() to know if any errors exist. 

```php

@if ($errors->any())
	@foreach($errors->all() as $error)
		<p>{{ $error }}</p>
	@endforeach
@endif

// You can also use @error and pass in the name of an input field to only do something when that field gives back an error like this
@error('title')
	<p> {{ $message }} </p>
@enderror
```

Your browser supports both "get" and "post" requests natively however most web frameworks also support additional request methods. Two of them are:
* Patch
* Delete
```php
Route::patch('', function () {

});


Route::delete('', function () {

});
```

**Unique tidbit:** You can attach a button into a totally different form by giving the form's id to its form attribute.

```php
<button form="xform">Submit</button>

<form id="xform">

</form>
```

# Route Model Binding

Laravel has a feature where for wildcards, instead of an id, we can give exactly the model that we desire. For instance, we can give a route a Job model and it will detect that automatically.

```php
Route::get('/jobs/{job}', function (Job $job) {
	return view('jobs.show', [
		'job' => $job
	]);
});
```

A few rules need to be abided:

* The wild card has to be named as exactly the variable parameter of the function.
* The anonymous function's varaible needs to have an explicit variable type declaration.

## Controllers

We've gone through Models, we've also gone through views but we still have not talked about the C in MVCs, That is **Controllers**.

In full-scale applications, controllers are the ones that process data and update views and models. Therefore it is not wise to delegate that job to the Routes in web.php.

You can create a controller class like this:

```shell
$ php artisan make:controller JobController
```

Then if you want to route the URI to the Controller:

```php
Route::get('/jobs', [JobController::class, '<name of action>'])
```

Actions are basically the functions that the controller has. The most common [[CRUD]] actions are:

* index: the homepage of the resource in general.
* show: Get action that shows a view for the models details
* edit: Get action that shows an edit view
* create: get action that shows a create view
* store: Post action that creates and stores a model instance
* update: Patch action that updates a model instance
* destroy: Delete action that deletes a model instance

You can also group these routes inside a Route::controller like this:

```php
Route::controller(JobController::class)->group(function () {
	Route::get('/jobs', 'index');
	Route::get('/jobs/create', 'create');
	Route::get('/jobs/{job}', 'show');
	Route::get('/jobs/{job}/edit', 'edit');
	Route::post('/jobs', 'store');
	Route::patch('/jobs/{job}', 'update');
	Route::delete('/jobs/{job}', 'destroy');
});
```

For views that are static and can never change, you can simply use Route::view.

```php
Route::view('/', 'home');
```
# Users and Authentication

Laravel makes dealing with user authentication really simple.

The way authentication generally works is that while registered, a user is created. Then when the time comes for the user to sign in, a session is created inside the session table and the user is given a session token. For security reasons, it is generally recommended to regenerate this session token on each login.

the @auth and @guest are blade directives that indicate code that need to be performed while the viewer is an authenticated user (@auth) or a guest user (@guest).

```php
@auth
<form method="POST" action="/logout">
	@csrf
	<x-form-button>Logout</x-form-button>
</form>
@endauth

@guest
	<x-nav-link href="/login">Login</x-nav-link>
	<x-nav-link href="/register">Register</x-nav-link>
@endguest
```

**Some Tidbits:**
* after validating user input: request()->validated(\[]), it returns an array of the validated items, assuming that the validated items' keys are the same as their database counterpart, you can send them directly to the Model's create function.
* the Illuminate\\Support\\Facades\\Auth class deals with authentications automatically. Use Auth::Login which takes in a user model to login an existing user. Auth::logout also logs out the user. 
* **it's frowned upon to use a get request to logout a loggedin user. That is usually done by a POST request**

There are 2 general ways to authorize a user for certain jobs:
### Gates

Gates are as the name suggests, gates that either allow or not allow a user to do certain things. 
Illuminate\\Support\\Facades\\Gates is a laravel class that handles such things.

For instance, for being able to edit a job, the user has to own an employer than has that job. We have to define a gate like this:

```php
// user is given by the session, if the viewer is a guest, then this gate will also never allow the given action
Gate::define('edit-job', function (User $user, Job $job) {
	return $job->employer->user->is($user);
});
```

You can then use Gate::Authorize('edit-job', $job), giving in an action and a job model. You can also use Gate::allowed or Gate::denied to get boolean expressions.

* These gates are ordinarily defined inside our AppServiceProvider in order to give global access to this action.
```php
public function boot(): void {
	Gate::define('edit-job', function (User $user, Job $job) {
		return $job->employer->user->is($user);
	});
}
```

You can also ideally use the can() or cannot() method for the same thing.

```php
Auth::user()->can('edit-job', $job); // edit-job action is defined by our gate.
```

### Middleware

### Policy

You can create policies for models in order to define what can or can't be done to them. This is used for most non-trivial applications.

```shell
$ php artisan make:policy
```

```php
class JobPolicy {
	public function edit(User $user, Job $job): bool {
		return $job->employer->user->is($user);
	}
}
```

Once you've defined a function, you can use that function's name as an action verb to supply can() and cannot() functions like before.

# Mailable Classes

In order to be able to mail to the user, we need mailable classes

```shell
$ php artisan make:mail
```

You'll have to change the content of your mailable class to return a view.

By default, the mail is sent to a log file. If you want the mail to be sent to a real email, you'll have to use third party mail services and you'll have to set it up inside your .env file.
[Here is an easy mailing service](https://mailtrap.io)

Whenever you want to send an email, you can do this:

```php
// ordinarily to() has to get an email address, however if your model has an email address, sending in the model itself does the job as well
Iluminate\Support\Facades\Mail::to($user)->send(new JobPosted());
```


# Queues

Sending Emails is great and all, but it is a slow process and can halt the entire website for a while. In order to avoid doing that, we can use a Queue.

laravel has a queue system that handles jobs asynchronous from the website's activities.

These jobs are held within the jobs table in the database. 

By executing this command, all jobs held within the jobs table will be executed.

```Shell
$ php artisan queue:work
```

In production, you can use services that handle queue's automatically.

dispatch() is a function that takes in an anonymous function. Its purpose is to call these functions through a queue.

```php
dispatch(function () {
	logger("Hi! This has been written asynchronously through a queue");
});
```

**By chaining a delay() function and giving it a number of seconds, it will delay its execution by that amount.**

# Dedicated Job Classes

You can dedicate a Job class to the handling of jobs. 

```php
$ php artisan make:job
```

```php
class TranslateJob implements ShouldQueue// this trait will make this job be something a queue can execute
{
use Queueable;

/**

* Create a new job instance.

*/

public function __construct(public Job $joblisting)
{
//
}
/**

* Execute the job.

*/
// this method is where your dispatch logic will be located 
public function handle(): void
{
	logger('Translating ' . $this->joblisting->title);
}

}
```

You can give this job a variable to work off of if necessary. For instance a job listing.

```php
$job = Job::first();
TransalteJob::dispatch($job);// you still use the dispatch method to handle its execution
```

# Unit Tests

Laravel comes with a default unit testing framework called Pest which is a wrapper on PHPUnit.

You can create a unit test by calling:

```shell
$ php artisan make:test JobTest
```

Inside a method of a unit test, you are generally going to follow these steps:

* Arrange: Arrange the world and states that you want to test under
* Act: Do an action
* Assert: Check if things change they way you had expected

**add this line to Pest.php to be able to unit test successfuly**

```php
uses(
	Tests\TestCase::class,
	Illuminate\Foundation\Testing\RefreshDatabase::class,
)->in('Feature', 'Unit');
```