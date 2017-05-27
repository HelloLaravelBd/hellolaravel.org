---
title: কন্ট্রোলার -  Controller
type:  হ্যালো লারাভেল ৫ । ৪
order: 7
---

## কন্ট্রোলার -  Controller

কন্ট্রোলার -  Controller হচ্ছে [MVC]() এর অংশ ।  এখানে Controller নিয়ে কিছু কথাবার্তা চলবে -
Controller খুজে পাবেন ` app/Http/Controllers` ডিরেক্টরি তে ।

Controller কথা যেখানে  `CRUD` সেখানে থাকে । `CRUD` স্টাইল `Controller` পারলেই আপণী যেকোনো Controller পারবেন ।

`php artisan make:controller PhotoController --resource`

একই সাথে [Model]() বানাতে চাইলে ...

`php artisan make:controller PhotoController --resource --model=Photo`

### CRUD পরিবার

| Verb  | URI  | Action  | Route Name  |
|---|---|---|---|
| GET  | `/photos`  |  index |  photos.index |
| GET  | `/photos/create	`| create | photos.create  |
|  POST | `/photos	`  | store  |  photos.store |
|  GET | `/photos/{photo}`  | show  |photos.show   |
| GET  |`/photos/{photo}/edit	`   |  edit |photos.edit   |
| PUT/PATCH	  | `/photos/{photo}	`  |  update |photos.update   |
| DELETE  |  `/photos/{photo}	` |  destroy |   photos.destroy|


php artisan make:controller PhotoController --resource কমান্ড করলে public function index ,create, store , edit , update , destroy তৈরি হবে ।  প্রথম কাজ হচ্ছে create , এখানে  ফর্ম  বানাবেন । দ্বিতীয় ধাপ store , এখানে ডাটার কাজ করবেন । এখানে ৩ তা কাজ করতে হবে ডাটা ভ্যালিডেট , স্টোর ডাটাবেস তারপর রিডিরেক্ট ।

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Post;
class PostController extends Controller
{
// All code here . 
}
```

```php
public function index()
{
  $posts = Post::orderBy('id', 'desc')->paginate(10);
  return view('posts.index')->withPosts($posts);
}

```

```php
public function create()
    {
        $post = Post::find($id);
        return view('posts.edit')->withPost($post);
    }
```

```php
public function store(Request $request)
{
    // Validate the data

    $this->validate($request, [
                'title'         => 'required|max:255',
                'slug'          => 'required|alpha_dash|min:5|max:255|unique:posts,slug',
                'category_id'   => 'required|integer',
                'body'          => 'required'
            ]);

    //store in the Database

    $post = new Post;
    $post->title = $request->title;
    $post->slug = $request->slug;
    $post->category_id = $request->category_id;
    $post->body = $request->body;
    $post->save();

    // Redirect  to another page
    return view('posts.show')->withPost($post);
}
```


```php
public function edit($id)
    {
        $post = Post::find($id);
        return view('posts.edit')->withPost($post);
    }
```

```php
public function update(Request $request, $id)
{
    // Validate the data

    $this->validate($request, [
                'title'         => 'required|max:255',
                'slug'          => 'required|alpha_dash|min:5|max:255|unique:posts,slug',
                'category_id'   => 'required|integer',
                'body'          => 'required'
            ]);

    //store in the Database

    $post = Post::find($id)
    $post->title = $request->title;
    $post->slug = $request->slug;
    $post->category_id = $request->category_id;
    $post->body = $request->body;
    $post->save();

    // Redirect  to another page
    return view('posts.show')->withPost($post);
}
```


```php

public function destroy($id)
{
    $post = Post::find($id);
    $post->delete();
}

```



edit হচ্ছে create এর অনুরপ , update ও store এর অনুরুপ ।

 * >>CRUD

```css
C ( create) = create / store
R ( Read) = index
U ( Update) = edit / update
D (Delete/Destroy) = destroy
```

[Route]() এর বিস্তারিত একটা আংশ থাকবে । এখানে resource প্রসঙ্গে একটু তুলে দরা হল

`Route::resource('photos', 'PhotoController'); `

এই একটা দিয়ে সব এক্সেস হবে ।   যদি `resource Controller` সহ  অন্য Controller থাকেে  তাহলে  `Route::resource('photos', 'PhotoController');` এর পরে [Route]() মেনে কাজ করতে হবে ।

```php
Route::get('photos/popular', 'PhotoController@method');

Route::resource('photos', 'PhotoController');
```


* শুধুমাত্র `index` , `show` এক্সেস করতে চাইলে only

```php
Route::resource('photo', 'PhotoController', ['only' => [
    'index', 'show'
]]);
```

একই ফলাফল এখানে except দিয়ে

```php
Route::resource('photo', 'PhotoController', ['except' => [
    'create', 'store', 'update', 'destroy'
]]);
```
