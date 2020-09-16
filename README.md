# Explore-Laravel

Exploring and learning Laravel, an MVC web framework.

## Installation

I used Composer to install Laravel. At first I installed XAMPP for PHP. Then I downloaded the executable installation file from composer website, as I'm in windows. The exe file automatically add the bin folder to the path. Then I ran this command to install Laravel globally:

```sh
composer global require laravel/installer
```

Now to create a project I have to run:

```sh
laravel new project-name
```

After the project is generated we can run the server by running:

```sh
php artisan serve
```

We can serve the projects easily by installing Valet using the composer. But valet isn't working in my machine right now.

## Routing

All of the routers are available in the `/routes` folder. To setup any web route, we can add a route in the `/routes/web.php` file like this:

```php
Route::get('/url', function () {
    return view('welcome');
});
```

This will setup the get request to the `/url` to return the view written in `/resources/views/welcome.blade.php`. 

To get the variables from a url like `example.com/something?name="KhanShaheb"` we can do:

```php
Route::get('/something', function () {
    $name = request('name');

    return view('home', [
        'name' => $name
    ]);
});
```

And that is how we can pass a variable to the view too.

For a wildcard url we can do:

```php
Route::get('/todo/{title}', function ($title) {
    return view('home', [
        'title' => $title
    ]);
});
```

This will get the requests like `example.com/todo/anything` and pass the title (here `anything`) to the view.

To connect a route to a controller we can do:

```php
// Import the controller
use App\Http\Controllers\PostsController;

Route::get('/posts/{id}', [PostsController::class, 'show']);
```

Here `PostsController` is the name of the controller and `show` is the function inside the controller that will be called once the url recieves a get request.

> [Create a controller](#controller)

## View

We can create files inside the `/resources/view` folder to create a new view file. To create a view named `home` we can create `home.blade.php`

> Blade is a simple templating engine for Laravel.

To get a variable inside a blade file we can do:

```php
<?= $var_name; ?>
```

But this won't sanitize the value of the variable. If we show something that is a input from the user we must sanitize the data. To sanitize we can simply do `{{ $var_name }}`. The Laravel will handle the sanitization part.

We can do `{!! $var_name !!}` to show the data without sanitization.

## Controller 

We can create a controller in two different ways. We can create a php file inside the `/app/Http/Controllers` folder. Or we can run:

```sh
php artisan make:controller controller_name
```

## Database

The description of the database should be written in the `.env` file. The `.env` file is generated during the project generation.

The config file for database connection is in the `config/database.php` file.

Now we can make queries to the database by using the `DB` namespace, like:

```php
$post = DB::table('posts')->where('id', '=', $id)->first();
```

More queries are available [here](https://laravel.com/docs/8.x/queries).

We can use **Eloquent** for cleaner queries and business logic. To create a Eloquent model we have to run:

```sh
php artisan make:model Post
```

Now we can do a `where` query like this:

```php
$post = Todo::where('id', '=', $id)->firstOrFail();
```

This will automatically send a 404 if the post is not available.