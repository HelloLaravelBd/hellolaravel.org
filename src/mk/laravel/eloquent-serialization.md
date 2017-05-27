---
title: Eloquent Serialization
type: laravel 5.4
order: 10.5
---

## Introduction

When building JSON APIs, you will often need to convert your models and relationships to arrays or JSON. Eloquent includes convenient methods for making these conversions, as well as controlling which attributes are included in your serializations.

<a name="serializing-models-and-collections"></a>
## Serializing Models & Collections

<a name="serializing-to-arrays"></a>
### Serializing To Arrays

To convert a model and its loaded [relationships](/mk/laravel/eloquent-relationships.html) to an array, you should use the `toArray` method. This method is recursive, so all attributes and all relations (including the relations of relations) will be converted to arrays:
```php
    $user = App\User::with('roles')->first();

    return $user->toArray();
```
You may also convert entire [collections](/mk/laravel/eloquent-collections.html) of models to arrays:
```php
    $users = App\User::all();

    return $users->toArray();
```
<a name="serializing-to-json"></a>
### Serializing To JSON

To convert a model to JSON, you should use the `toJson` method. Like `toArray`, the `toJson` method is recursive, so all attributes and relations will be converted to JSON:
```php
    $user = App\User::find(1);

    return $user->toJson();
```
Alternatively, you may cast a model or collection to a string, which will automatically call the `toJson` method on the model or collection:
```php
    $user = App\User::find(1);

    return (string) $user;
```
Since models and collections are converted to JSON when cast to a string, you can return Eloquent objects directly from your application's routes or controllers:
```php
    Route::get('users', function () {
        return App\User::all();
    });
```
<a name="hiding-attributes-from-json"></a>
## Hiding Attributes From JSON

Sometimes you may wish to limit the attributes, such as passwords, that are included in your model's array or JSON representation. To do so, add a `$hidden` property to your model:
```php
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class User extends Model
    {
        /**
         * The attributes that should be hidden for arrays.
         *
         * @var array
         */
        protected $hidden = ['password'];
    }
```
> {note} When hiding relationships, use the relationship's method name, not its dynamic property name.

Alternatively, you may use the `visible` property to define a white-list of attributes that should be included in your model's array and JSON representation. All other attributes will be hidden when the model is converted to an array or JSON:
```php
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class User extends Model
    {
        /**
         * The attributes that should be visible in arrays.
         *
         * @var array
         */
        protected $visible = ['first_name', 'last_name'];
    }
```
#### Temporarily Modifying Attribute Visibility

If you would like to make some typically hidden attributes visible on a given model instance, you may use the `makeVisible` method. The `makeVisible` method returns the model instance for convenient method chaining:
```php
    return $user->makeVisible('attribute')->toArray();
```
Likewise, if you would like to make some typically visible attributes hidden on a given model instance, you may use the `makeHidden` method.
```php
    return $user->makeHidden('attribute')->toArray();
```
<a name="appending-values-to-json"></a>
## Appending Values To JSON

Occasionally, when casting models to an array or JSON, you may wish to add attributes that do not have a corresponding column in your database. To do so, first define an [accessor](/mk/laravel/eloquent-mutators.html) for the value:
```php
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class User extends Model
    {
        /**
         * Get the administrator flag for the user.
         *
         * @return bool
         */
        public function getIsAdminAttribute()
        {
            return $this->attributes['admin'] == 'yes';
        }
    }
```
After creating the accessor, add the attribute name to the `appends` property on the model. Note that attribute names are typically referenced in "snake case", even though the accessor is defined using "camel case":
```php
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class User extends Model
    {
        /**
         * The accessors to append to the model's array form.
         *
         * @var array
         */
        protected $appends = ['is_admin'];
    }
```
Once the attribute has been added to the `appends` list, it will be included in both the model's array and JSON representations. Attributes in the `appends` array will also respect the `visible` and `hidden` settings configured on the model.
