---
title: HTTP Responses
type: laravel 5.4
order: 5.6
---

## Creating Responses

#### Strings & Arrays

All routes and controllers should return a response to be sent back to the user's browser. Laravel provides several different ways to return responses. The most basic response is simply returning a string from a route or controller. The framework will automatically convert the string into a full HTTP response:
```php
    Route::get('/', function () {
        return 'Hello World';
    });
```
In addition to returning strings from your routes and controllers, you may also return arrays. The framework will automatically convert the array into a JSON response:
```php
    Route::get('/', function () {
        return [1, 2, 3];
    });
```
 Did you know you can also return [Eloquent collections](/mk/laravel/eloquent-collections.html) from your routes or controllers? They will automatically be converted to JSON. Give it a shot!

#### Response Objects

Typically, you won't just be returning simple strings or arrays from your route actions. Instead, you will be returning full `Illuminate\Http\Response` instances or [views](/mk/laravel/views.html).

Returning a full `Response` instance allows you to customize the response's HTTP status code and headers. A `Response` instance inherits from the `Symfony\Component\HttpFoundation\Response` class, which provides a variety of methods for building HTTP responses:
```php
    Route::get('home', function () {
        return response('Hello World', 200)
                      ->header('Content-Type', 'text/plain');
    });
```
<a name="attaching-headers-to-responses"></a>
#### Attaching Headers To Responses

Keep in mind that most response methods are chainable, allowing for the fluent construction of response instances. For example, you may use the `header` method to add a series of headers to the response before sending it back to the user:

```php
    return response($content)
                ->header('Content-Type', $type)
                ->header('X-Header-One', 'Header Value')
                ->header('X-Header-Two', 'Header Value');
```
Or, you may use the `withHeaders` method to specify an array of headers to be added to the response:
```php
    return response($content)
                ->withHeaders([
                    'Content-Type' => $type,
                    'X-Header-One' => 'Header Value',
                    'X-Header-Two' => 'Header Value',
                ]);
```
<a name="attaching-cookies-to-responses"></a>
#### Attaching Cookies To Responses

The `cookie` method on response instances allows you to easily attach cookies to the response. For example, you may use the `cookie` method to generate a cookie and fluently attach it to the response instance like so:
```php
    return response($content)
                    ->header('Content-Type', $type)
                    ->cookie('name', 'value', $minutes);
```
The `cookie` method also accepts a few more arguments which are used less frequently. Generally, these arguments have the same purpose and meaning as the arguments that would be given to PHP's native [setcookie](https://secure.php.net/manual/en/function.setcookie.php) method:
```php
    ->cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)
```
<a name="cookies-and-encryption"></a>
#### Cookies & Encryption

By default, all cookies generated by Laravel are encrypted and signed so that they can't be modified or read by the client. If you would like to disable encryption for a subset of cookies generated by your application, you may use the `$except` property of the `App\Http\Middleware\EncryptCookies` middleware, which is located in the `app/Http/Middleware` directory:
```php
    /**
     * The names of the cookies that should not be encrypted.
     *
     * @var array
     */
    protected $except = [
        'cookie_name',
    ];
```
<a name="redirects"></a>
## Redirects

Redirect responses are instances of the `Illuminate\Http\RedirectResponse` class, and contain the proper headers needed to redirect the user to another URL. There are several ways to generate a `RedirectResponse` instance. The simplest method is to use the global `redirect` helper:
```php
    Route::get('dashboard', function () {
        return redirect('home/dashboard');
    });
```
Sometimes you may wish to redirect the user to their previous location, such as when a submitted form is invalid. You may do so by using the global `back` helper function. Since this feature utilizes the [session](/mk/laravel/session.html), make sure the route calling the `back` function is using the `web` middleware group or has all of the session middleware applied:
```php
    Route::post('user/profile', function () {
        // Validate the request...

        return back()->withInput();
    });
```
<a name="redirecting-named-routes"></a>
### Redirecting To Named Routes

When you call the `redirect` helper with no parameters, an instance of `Illuminate\Routing\Redirector` is returned, allowing you to call any method on the `Redirector` instance. For example, to generate a `RedirectResponse` to a named route, you may use the `route` method:
```php
    return redirect()->route('login');
```
If your route has parameters, you may pass them as the second argument to the `route` method:
```php
    // For a route with the following URI: profile/{id}

    return redirect()->route('profile', ['id' => 1]);
```
#### Populating Parameters Via Eloquent Models

If you are redirecting to a route with an "ID" parameter that is being populated from an Eloquent model, you may simply pass the model itself. The ID will be extracted automatically:
```php
    // For a route with the following URI: profile/{id}

    return redirect()->route('profile', [$user]);
```
If you would like to customize the value that is placed in the route parameter, you should override the `getRouteKey` method on your Eloquent model:
```php
    /**
     * Get the value of the model's route key.
     *
     * @return mixed
     */
    public function getRouteKey()
    {
        return $this->slug;
    }
```
<a name="redirecting-controller-actions"></a>
### Redirecting To Controller Actions

You may also generate redirects to [controller actions](/mk/laravel/controllers.html). To do so, pass the controller and action name to the `action` method. Remember, you do not need to specify the full namespace to the controller since Laravel's `RouteServiceProvider` will automatically set the base controller namespace:
```php
    return redirect()->action('HomeController@index');
```
If your controller route requires parameters, you may pass them as the second argument to the `action` method:
```php
    return redirect()->action(
        'UserController@profile', ['id' => 1]
    );
```
<a name="redirecting-with-flashed-session-data"></a>
### Redirecting With Flashed Session Data

Redirecting to a new URL and [flashing data to the session](/mk/laravel/session.html#flash-data) are usually done at the same time. Typically, this is done after successfully performing an action when you flash a success message to the session. For convenience, you may create a `RedirectResponse` instance and flash data to the session in a single, fluent method chain:
```php
    Route::post('user/profile', function () {
        // Update the user's profile...

        return redirect('dashboard')->with('status', 'Profile updated!');
    });
```
After the user is redirected, you may display the flashed message from the [session](/mk/laravel/session.html). For example, using [Blade syntax](/mk/laravel/blade.html):
```php
    @if (session('status'))
        <div class="alert alert-success">
            {{ session('status') }}
        </div>
    @endif
```
<a name="other-response-types"></a>
## Other Response Types

The `response` helper may be used to generate other types of response instances. When the `response` helper is called without arguments, an implementation of the `Illuminate\Contracts\Routing\ResponseFactory` [contract](/mk/laravel/contracts.html) is returned. This contract provides several helpful methods for generating responses.

<a name="view-responses"></a>
### View Responses

If you need control over the response's status and headers but also need to return a [view](/mk/laravel/views.html) as the response's content, you should use the `view` method:
```php
    return response()
                ->view('hello', $data, 200)
                ->header('Content-Type', $type);
```
Of course, if you do not need to pass a custom HTTP status code or custom headers, you should use the global `view` helper function.

<a name="json-responses"></a>
### JSON Responses

The `json` method will automatically set the `Content-Type` header to `application/json`, as well as convert the given array to JSON using the `json_encode` PHP function:
```php
    return response()->json([
        'name' => 'Abigail',
        'state' => 'CA'
    ]);
```
If you would like to create a JSONP response, you may use the `json` method in combination with the `withCallback` method:
```php
    return response()
                ->json(['name' => 'Abigail', 'state' => 'CA'])
                ->withCallback($request->input('callback'));
```
<a name="file-downloads"></a>
### File Downloads

The `download` method may be used to generate a response that forces the user's browser to download the file at the given path. The `download` method accepts a file name as the second argument to the method, which will determine the file name that is seen by the user downloading the file. Finally, you may pass an array of HTTP headers as the third argument to the method:
```php
    return response()->download($pathToFile);

    return response()->download($pathToFile, $name, $headers);

    return response()->download($pathToFile)->deleteFileAfterSend(true);
```
> {note} Symfony HttpFoundation, which manages file downloads, requires the file being downloaded to have an ASCII file name.

<a name="file-responses"></a>
### File Responses

The `file` method may be used to display a file, such as an image or PDF, directly in the user's browser instead of initiating a download. This method accepts the path to the file as its first argument and an array of headers as its second argument:
```php
    return response()->file($pathToFile);

    return response()->file($pathToFile, $headers);
```
<a name="response-macros"></a>
## Response Macros

If you would like to define a custom response that you can re-use in a variety of your routes and controllers, you may use the `macro` method on the `Response` facade. For example, from a [service provider's](/mk/laravel/providers.html) `boot` method:
```php
    <?php

    namespace App\Providers;

    use Illuminate\Support\ServiceProvider;
    use Illuminate\Support\Facades\Response;

    class ResponseMacroServiceProvider extends ServiceProvider
    {
        /**
         * Register the application's response macros.
         *
         * @return void
         */
        public function boot()
        {
            Response::macro('caps', function ($value) {
                return Response::make(strtoupper($value));
            });
        }
    }
```
The `macro` function accepts a name as its first argument, and a Closure as its second. The macro's Closure will be executed when calling the macro name from a `ResponseFactory` implementation or the `response` helper:
```php
    return response()->caps('foo');
```
