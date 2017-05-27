---
title: ডাটাবেস আলাপ - (Database)
type:  হ্যালো লারাভেল ৫ । ৪
order: 5
---
---
হ্যালো লারাভেল !

## ডাটাবেস (Database) কি ?

 সবাই কম বেশি ডাটা কথাটা শুনেছেন । ডাটা মানেই সেখানে কিছু তথ্যের কারবার । ডাটাবেস টা হচ্ছে আপনার যাবতীয় তথ্য সংগ্রহশালা । অন্য ভাবে বললে ডাটাবেস হচ্ছে  schemas, tables, queries, reports এসবের কালেকশন ।



### কোন ডাটাবেস ব্যবহার করবো ?

লারাভেল চারটা ডাটাবেস কে পরিচয় করিয়ে দিয়েছে

* MySQL
* Postgres
* SQLite
* SQL Server

আপনি যে ডাটাবেস ভাল লাগে সেটা ব্যবহার করতে পারেন । আপনি লারাভেল প্রোজেক্ট এর ডাটাবেস কনফিগারেশন পাবেন config/database.php তে ।


### ডাটাবেস সেটআপ পদ্ধতিঃ

# `SQLite কনফিগারেশন -`

SQLite সেটআপ করতে আপনাকে প্রথমে কম্যান্ড করতে হবে `touch database/hello_laravel.sqlite` । তাহলে আপনার

`database` ফোল্ডার এ `hello_laravel.sqlite`  নামে একটা ফাইল দেখতে পাবেন । এই ফাইল টা আপনার sqlite এ যুক্ত করতে হবে ।

এখন লারাভেল প্রোজেক্ট এর  `DB_CONNECTION এ  দেন sqlite` , `DB_DATABASE` এ `database/hello_laravel.sqlite`

```php
DB_CONNECTION=sqlite
DB_DATABASE=database/hello_laravel.sqlite
```

## ডাইনামিক ডাটা টেবিল তৈরি / মাইগ্রেশন

Vue does provide a more generic way to observe and react to data changes on a Vue instance: **watch properties**. When you have some data that needs to change based on some other data, it is tempting to overuse `watch` - especially if you are coming from an AngularJS background. However, it is often a better idea to use a computed property rather than an imperative `watch` callback. Consider this example:
### মাইগ্রেশন তৈরি
মাইগ্রেশন এর জন্য টার্মিনাল গিয়ে হিট করুন ।
``` html
php artisan make:migration create_posts_table --create=posts

```
এখন
* database
   * migrations
      * 2017_02_06_175142_create_posts_table.php
গিয়ে এ  এমন একটা ফাইল পাবেন । আপনি ডাটা কি কি রাখবেন সেটা সেট আপ দিবেন।


  <p class="tip"> মনে রাখবেন টেবিল নাম সব সময় বহু বচন হবে । যেমন  `users` `posts` `tags ` ।। </p>
``` php
    <?php
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
class CreatePostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->text('body');
            $table->timestamps();
        });
    }
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('posts');
    }
}

```
মাইগ্রেশন এ দুইটা মেথড থাকে `up`   এবং  `down`

এখন ডাটাবেস এ কমান্ড এর মাধ্যমে টেবিল এড করার পালা , টার্মিনাল এ হিট করুন
```php
   php artisan migrate
```
 কোন এরর না আসলে আপনি সফলভাবে টেবিল  যুক্ত করতে পারছেন । আপনার প্যানেল এ যাচাই করুন  `users` নামে টেবিল যুক্ত হয়েছে  এবং তথ্য গুলো সব আছে ।

###  নতুন কলাম যুক্ত করা

  *  আপনার নামে একটা টেবিল আছে , এখন আপনি ওই টেবিল আরও কিছু তথ্য যুক্ত করতে চাচ্ছেন , তাহলে টার্মিনালে হিট করুন

``` html

php artisan make:migration add_category_id_to_posts --table=posts

```

 category_id   যুক্ত হবে posts টেবিলে ।

 ```php
<?php
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
class AddCategoryIdToPosts extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->integer('category_id')->nullable()->after('slug')->unsigned();
        });
    }
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropColumn('category_id');
        });
    }
}
```



এখন  ডাটাবেস এ কমান্ড এর মাধ্যমে আগের টেবিল  এ নতুন কলাম এড করার পালা , টার্মিনাল এ হিট করুন

```php

   php artisan migrate

```
ডাটাবেস যাচাই করুন `slug` পরে `category_id` কলাম যুক্ত  হয়েছে

The above code is imperative and repetitive. Compare it with a computed property version:

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Much better, isn't it?

### কলাম টাইপ

|  Command | Command  | Command  |
|--:|---|---|
| `$table->bigIncrements('id');`  | `$table->bigInteger('votes')`;	  |  `$table->binary('data');`	  |
| `$table->boolean('confirmed');	`  | `$table->char('name', 4);	`  | `$table->date('created_at');	`  |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |



## Query Builder

### database tips:

 * `$users = DB::table('users')->get();`

 * `$users = App\User::all();` / `$users = User::all();`


### Retrieving Results

 * Retrieving All Rows From A Table

 ডাটাবেসের সকল সারির ডাটার জন্য নিচের কোড অনুসরন করতে হবে । $users = DB::table('users')->get();  $users একটা ভারিয়েবল , DB::table('users') এর users ডাটাবেস টেবিল নাম । DB:: হচ্ছে লারাভেল এর [facade]() । DB:: , UserController এ ব্যবহার করার জন্য use Illuminate\Support\Facades\DB; প্রথমে বলে দিতে হবে । get(); [] মেথড দিয়ে সব ডাটা পাবেন ।


 ```php
 <?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\DB;
use App\Http\Controllers\Controller;
use DB;
class UserController extends Controller
{
    /**
     * Show a list of all of the application's users.
     *
     * @return Response
     */
    public function index()
    {
        $users = DB::table('users')->get();

        return view('user.index', ['users' => $users]);
    }
}
```

* মডেল এর মাধ্যমে ডাটা সংগ্রহঃ

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\DB;
use App\Http\Controllers\Controller;
use App\User;
class UserController extends Controller
{
   /**
    * Show a list of all of the application's users.
    *
    * @return Response
    */
   public function index()
   {
       $users = User::all();

       return view('user.index', ['users' => $users]);
   }
}
```


### Chunking Results


### Aggregates


### Selects

সব সময় ডাটা টেবিল এর সকল কলাম এর দরকার পরে না । যে যে কলাম দেখাতে চান সেগুলো select() মেথড দিয়ে ফিক্স ট করে দিতে হবে । যেমনঃ সকল user এর name এবং email

```php
$users = DB::table('users')->select('name', 'email')->get();
```

* distinct - (স্বতন্ত্র) ব্যবহার করলে কোন মাল্টিপল একই ডাটা একটা ডাটা হিসেবে শো করবে । যেমনঃ  users এর দেশ এর লিস্ট    $users->country  । users দের দেশ বাংলাদেশ যতবার এ থাকুক সেটা একবার ই দেখাবে ।

```php
$users = DB::table('users')->distinct()->get();
```

### Joins

একটা টেবিল কে অন্য টেবিল এর সাথে সংযুক্ত করার জন্য join() ব্যবহার করতে পারেন ।Inner Join ,Left Join , right Join  সবগুলোর কাজ একই । Inner Join  এর মাধ্যমে  users টেবিল এর সাথে contacts , orders টেবিল সংযুক্ত । এই কাজ তা আপনি চাইলে ফরেন কে দিয়েও করতে পারেন ।

```php
$users = DB::table('users')
            ->join('contacts', 'users.id', '=', 'contacts.user_id')
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.*', 'contacts.phone', 'orders.price')
            ->get();

```
###  Where  package

Where সম্পর্কে একটা গল্প বলি । সামিনা আর রুবি দুই বান্ধবী । বন্ধু বান্ধব রা তো আড্ডা চলে । একদিন আড্ডার ফাকে সামিনা রুবি কে বলতাছে দোস্ত ঢাকাতে তুই অনেকদিন যাবত আছিস , ঢাকার ভিতথা ঐতিহাসিক কোথায় গেছিস । রুবি বলল লালবাগ কেল্লা , আহসান মঞ্জিল , পানাম নগর । রুবি কিন্তু মহাস্থানগড় বা ঢাকার বাইরের  জায়গার নাম বলে নি , কারন সামিনা বলেছে ঢাকার  ভিতরের জায়গা ।

Where এ  কিছু  শর্ত দিয়ে দেওয়া হয় , ফলাফল শর্ত মেনে চলে । Where এ তিনটি আর্গুমেন্ট থাকে কলামের নাম  , অপারেটর (=, <=,>=) , ভ্যালু

* যে যে user এর votes  সংখ্যা ১০০ এর সমান , শুধু সেসব user এর ডাটা দেখাবে ।

```php
$users = DB::table('users')->where('votes', '=', 100)->get();
```

যদি কোন অপারেটর ব্যবহার না করেন , সেটা অটোমেটিক সমান (=) অপারেটর হয়ে যাবে ।

```php
$users = DB::table('users')->where('votes', 100)->get();
```
* Where Array : দুই বা ততোধিক কলাম Where এ ব্যবহার করতে চাইলে অ্যারে করতে হবে । এসময় Where ,And  (&& ) এর মত কাজ করবে

```php
$users = DB::table('users')->where([
    ['status', '=', '1'],
    ['subscribed', '>', '100'],
])->get();
```

#  orWhere:

 যেকোনো  শর্ত মেনে চললে ফলাফল দেখাবে ।

```php
$users = DB::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();

```

# whereBetween

যেখানে user এর vote 100-500 , সেসব user এর ডাটা দেখাবে ।

```php
$users = DB::table('users')
                  ->whereBetween('votes', [100, 500])->get();
```

# whereNotBetween
whereNotBetween হচ্ছে whereBetween এর উল্টা । যেখানে user এর vote 100-500 , সেসব user এর ডাটা ছাড়া বাকি সব user এর ডাটা দেখাবে   ।

```php
$users = DB::table('users')
                    ->whereNotBetween('votes', [100, 500])
                    ->get();
```
### Ordering, Grouping, Limit, & Offset

# orderBy

orderBy এ দুইটা টপিক ascending (asc), descending (desc) । এই টপিক এ খুব মনে পরবে ক্লাস ওয়ান এর ছোট হতে বড় , বড় হতে ছোট ক্রমিক সংখ্যা সাজানো । ascending হচ্ছে ছোট হতে বড় সাজানো আর descending হচ্ছে বড় হতে ছোট সাজানো । asc করলে চলে আবার না করলেও চলে , অটোমেটিক সব asc আকারে দেখাবে । অনেক সময় descending আকারে সাজানোর প্রয়োজন হয় , যেমন কোন সাইট এর এডমিন যদি সকল পোস্ট দেখতে চান , তখন পোস্তগুলোকে desc এ সাজাতে হয় ।

```php
$users = DB::table('users')
                ->orderBy('name', 'desc')
                ->get();
```

# latest / oldest

```php
$user = DB::table('users')
                ->latest()
                ->first();
```

# skip / take

```php
$users = DB::table('users')->skip(10)->take(5)->get();
```

# offset / limit

```php
$users = DB::table('users')
                ->offset(10)
                ->limit(5)
                ->get();
```

### Updates

# update()

```php
DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);
```

# Increment & Decrement

```php
DB::table('users')->increment('votes');

DB::table('users')->increment('votes', 5);

DB::table('users')->decrement('votes');

DB::table('users')->decrement('votes', 5);
```

# Deletes

```php
DB::table('users')->delete();

DB::table('users')->where('votes', '>', 100)->delete();
```
