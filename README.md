Notes-Laravel
=============

Useful snippets from common tasks with the Laravel PHP framework.

## Colophon

Hi there! I have been taking some notes on common tasks I had to do using the [Laravel](http://www.laravel.com) framework. I will be updating as I learn new stuff or find useful tips.

## About

I am Nono Martínez Alonso, a Spanish architect currently working at Foster + Partners. I tweet at [@nonoesp](http://www.twitter.com/nonoesp) and write at [nono.ma/says](http://nono.ma/says). I would love to hear about it if you find this useful. Thanks!

## Further Reading

Articles from my blog related with Laravel.

* [Localizing a Laravel App](http://nono.ma/says/localizing-a-laravel-web-app)
* [Incoming Laravel Posts](http://nono.ma/says/incoming-laravel-posts)
* [Laravel Introduction Videos](http://nono.ma/says/laravel-introduction-videos) (Spanish)

# Notes

## Route

```php
// With alias and function
Route::get('page', array('as' => 'alias', function(){});
// With controller and alias
Route::get('page', array('as' => 'alias', 'uses' => 'SomeController@getPage'));
```

## Link

```php
HTML::link('articles/', 'Articles')
```

## Route URL with Parameters

```php
URL::route('articles.alias', array('id' => $id));
```

## Ignore a Subfolder

In the .htaccess file, assume we already have the RewriteRule to correctly display pretty URLs, then we have to add the following line with **/foldertoignore** being the name of the folder you want to behave as if Laravel was not installed into your server.

```php
<IfModule mod_rewrite.c>
	RewriteEngine On
	RewriteRule ^(.*)$ public/$1 [L]
	RewriteCond %{REQUEST_URI} !^/foldertoignore
</IfModule>
```

## Collection from Array
*Code adapted from [here](http://www.laravel-tricks.com/tricks/arrays-on-steroids).*

```php
$devs = [
['name' => 'Nono Martínez Alonso','email' => 'random@gmail.com'],
['name' => 'John Appleseed','email' => 'other@gmail.com']
];

// Convert array to collection

$devs = new Illuminate\Support\Collection( $devs );

//Now I can do something like :
$devs->first() // ['name' => 'Nono Martínez Alonso','email' => 'random@gmail.com']

$devs->last() // ['name' => 'John Appleseed','email' => 'other@gmail.com']

$devs->push(['name' => 'Bill Gates','email' => 'bill@microsoft.com']); // will add to collection
```

## Error Handling with Routes

The following example would handle the error *ModelNotFoundException,* which would be thrown by the Eloquent model.

```php
use Illuminate\Database\Eloquent\ModelNotFoundException;

App::error(function(ModelNotFoundException $e)
{
	return Response::make('Not Found', 404);
});
```

## Controller Filters

(from [Laravel 4.2 Controllers](http://laravel.com/docs/4.2/controllers))

### Controller route with Filter

```php
Route::get('profile', array('before' => 'auth', 'uses' => 'UserController@showProfile'));
```

### Filters from within a controller

```php
class UserController extends BaseController {

	public function __construct()
	{
		$this->beforeFilter('auth', array('except' => 'getLogin'));

		$this->beforeFilter('csrf', array('on' => 'post'));

		$this->afterFilter('log', array('only' => array('fooAction' => 'barAction')));
	}
}
```

### Send Email and Pass Arguments to the Closure

*Assume the $order variable has a value for $order->token, and the variable $recipient has a value $recipient->to and $recipient->email. Those values would have been set previously outside the closure. The mail body is created with a view, located at app/views/emails/my-view.blade.php—which will receive all the date we pass into the array as key-value.*

```php
Mail::send('emails.my-view', array('key' => 'value'), function($message) use ($order, $recipient) {
  $message->to($recipient->email, $recipient->name)->subject('Thank You for Your Order '.$order->token);
}
```

## License

Notes-MicroStation is licensed under the MIT license. (http://opensource.org/licenses/MIT)