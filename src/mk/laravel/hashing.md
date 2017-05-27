---
title: Hashing
type: laravel 5.4
order: 7.5
---

## Introduction

The Laravel `Hash` [facade](/mk/laravel/facades.html) provides secure Bcrypt hashing for storing user passwords. If you are using the built-in `LoginController` and `RegisterController` classes that are included with your Laravel application, they will automatically use Bcrypt for registration and authentication.

> {tip} Bcrypt is a great choice for hashing passwords because its "work factor" is adjustable, which means that the time it takes to generate a hash can be increased as hardware power increases.

<a name="basic-usage"></a>
## Basic Usage

You may hash a password by calling the `make` method on the `Hash` facade:
```php
    <?php

    namespace App\Http\Controllers;

    use Illuminate\Http\Request;
    use Illuminate\Support\Facades\Hash;
    use App\Http\Controllers\Controller;

    class UpdatePasswordController extends Controller
    {
        /**
         * Update the password for the user.
         *
         * @param  Request  $request
         * @return Response
         */
        public function update(Request $request)
        {
            // Validate the new password length...

            $request->user()->fill([
                'password' => Hash::make($request->newPassword)
            ])->save();
        }
    }
```
#### Verifying A Password Against A Hash

The `check` method allows you to verify that a given plain-text string corresponds to a given hash. However, if you are using the `LoginController` [included with Laravel](/mk/laravel/authentication.html), you will probably not need to use this directly, as this controller automatically calls this method:
```php
    if (Hash::check('plain-text', $hashedPassword)) {
        // The passwords match...
    }
```
#### Checking If A Password Needs To Be Rehashed

The `needsRehash` function allows you to determine if the work factor used by the hasher has changed since the password was hashed:
```php
    if (Hash::needsRehash($hashed)) {
        $hashed = Hash::make('plain-text');
    }
```
