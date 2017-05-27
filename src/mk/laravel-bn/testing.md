---
title: লারাভেল ডাস্ক -   Laravel Dusk
type: হ্যালো লারাভেল ৫ । ৪
order: 26
---


### Laravel Dusk

### ইন্সটলেশন
প্রথমে  কম্প্রজার দিয়ে প্যাকেজ টা ইন্সটল করে নিতে হবে ।

```php
composer require laravel/dusk

```
এখন  `AppServiceProvider` এ `use Laravel\Dusk\DuskServiceProvider;` রেজিস্টার করতে হবে ।

```php
use Laravel\Dusk\DuskServiceProvider;

/**
 * Register any application services.
 *
 * @return void
 */
public function register()
{
    if ($this->app->environment('local', 'testing')) {
        $this->app->register(DuskServiceProvider::class);
    }
}
```
এখন `dusk` ইন্সটল করতে হবে কম্যান্ড দিয়ে

```php
php artisan dusk:install
```
`tests` ডিরেক্টরি তে `Browser` ফোল্ডার তৈরি হয়েছে । `.env` ফাইল এ আপনার `APP_URL` দিন । যারা localhost ব্যবহার করেন  `APP_URL=localhost:8000` । কাজ শেষ । টেস্ট করুন । টেস্ট ব্রাউজার ChromeDriver । আপনি চাইলে পরিবর্তন করতে পারবেন ।


```php
php artisan dusk
```
এটা ডিফল্ট ExampleTest হতে টেস্ট । এখন নিজেদের মত করে টেস্ট করবেন ।

php artisan dusk:make RegisterTest


```php
<?php

namespace Tests\Browser;

use Tests\DuskTestCase;
use Laravel\Dusk\Browser;
use Illuminate\Foundation\Testing\DatabaseMigrations;

class RegisterTest extends DuskTestCase
{
    /**
     * A Dusk test example.
     *
     * @return void
     */
    public function testExample()
    {
        $this->browse(function (Browser $browser) {
            $browser->visit('/')
                    ->clickLink('Register')
                    ->assertSee('Register')
                    ->value('#name','Hello Laravel')
                    ->value('#email','hellolaravel@code4mk.com')
                    ->value('#password','123456')
                    ->value('#password-confirm','123456')
                    ->click('button[type="submit"]')
                    ->assertPathIs('/home')
                    ->assertSee("You are Loged in!");
        });
    }
}
```

```php
php artisan dusk
```
