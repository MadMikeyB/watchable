# Laravel Watchable

[![Packagist](https://img.shields.io/packagist/v/jamesmills/watchable.svg?style=for-the-badge)](https://packagist.org/packages/jamesmills/watchable)
![Packagist](https://img.shields.io/packagist/dt/jamesmills/watchable.svg?style=for-the-badge)
[![Travis](https://img.shields.io/travis/jamesmills/watchable.svg?style=for-the-badge)](https://travis-ci.org/jamesmills/watchable)
![Packagist](https://img.shields.io/packagist/l/jamesmills/watchable.svg?style=for-the-badge)
[![Buy us a tree](https://img.shields.io/badge/Treeware-%F0%9F%8C%B3-lightgreen?style=for-the-badge)](https://plant.treeware.earth/jamesmills/watchable)
[![Treeware (Trees)](https://img.shields.io/treeware/trees/jamesmills/watchable?style=for-the-badge)](https://plant.treeware.earth/jamesmills/watchable)

Enable users to watch various models in your application.
 - Designed to work with Laravel Eloquent models
 - Just add the trait to the model you would like to be watchable
 - Watches are unique for one model and one user
 - Events are fired on `watched` and `unwatched` methods
 - Built to work with Laravel Notifications

## Installation

Pull in the package using Composer

    composer require jamesmills/watchable

> **Note**: If you are using Laravel 5.5, the next step for provider are unnecessary. Laravel Watchable supports Laravel [Package Discovery](https://laravel.com/docs/5.5/packages#package-discovery).

Include the service provider within `app/config/app.php`.

```php
'providers' => [
    ...
    JamesMills\Watchable\WatchableServiceProvider::class,
],
```

Publish and run the database migrations

```bash
php artisan vendor:publish --provider="JamesMills\Watchable\WatchableServiceProvider" --tag="migrations"
php artisan migrate
```

## Sample Usage and Boilerplate

I wrote a blog post to give you some boilerplate code that you can use in your application to wrap around the Laravel Watchable package. 

https://jamesmills.co.uk/2017/10/22/laravel-watchable-package

## How to use

### Prepare your model to be watched

Simply add the `watchable` trait to your model

```php
use Illuminate\Database\Eloquent\Model;
use JamesMills\Watchable\Traits\Watchable;

class Book extends Model {
    use Watchable;
} 
```

### Available methods

Watch a model

```php
$book = Book::first();
$book->watch();  
```

Unwatch a model

```php
$book = Book::first();
$book->unwatch(); 
```

Toggle the watching of a model

```php
$book = Book::first();
$book->toggleWatch(); 
```

You can optionally send the ```$user_id``` if you don't want to use the built in ```auth()->user()->id``` functionality.

```php
$book = Book::first();
$book->watch($user_id);
$book->unwatch($user_id); 
$book->toggleWatch($user_id); 
```

Find out if the current user is watching the model

```php
@if ($book->isWatched())
    {{ You are watching this book }}
@else
    {{ You are NOT watching this book }}
@endif
```

Get a collection of the user who are watching a model

```php
$book = Book::first();
$book->collectWatchers(); 
```

### Use with Notifications

One of the main reasons I built this package was to scratch my own itch with an application I am building. I wanted to be able to send notifications to user who were watching a given model and I also wanted to allow users to be able to watch a number of different models.

```php
public function pause(Order $order)
{
    $this->performAction('paused', $order);
    Notification::send($order->collectWatchers(), new OrderPaused($order));
}
```

## License

This package is 100% free and open-source, under the [MIT license](LICENSE.md). Use it however you want.

This package is [Treeware](https://treeware.earth). If you use it in production, then we ask that you [**buy the world a tree**](https://plant.treeware.earth/jamesmills/watchable) to thank us for our work. By contributing to the Treeware forest you’ll be creating employment for local families and restoring wildlife habitats.
