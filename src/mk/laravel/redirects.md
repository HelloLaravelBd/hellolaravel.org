---
title: redirect
type: laravel 5.4
order: 11.6
---

## Creating Redirects

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
## Redirecting To Named Routes

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
## Redirecting To Controller Actions

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
## Redirecting With Flashed Session Data

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
