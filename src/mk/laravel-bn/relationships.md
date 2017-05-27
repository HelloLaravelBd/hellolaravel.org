---
title: রিলেশনশিপ
type:  হ্যালো লারাভেল ৫ । ৪
order: 6
---
---
হ্যালো লারাভেল !


##  রিলেশনশিপ কি?

আপনি রিলেশন অথবা আত্মীয়তার সাথে অবশ্যই জড়িত । একটা  অবজেক্ট এর সাথে     অন্য  অবজেক্ট যে সম্পর্ক  সেটাই হচ্ছে রিলেশিপ । লারাভেল এ একটা টেবিল এর সাথে  অন্য টেবিল এর সম্পর্ক রিলেশনশিপ ।

###  রিলেশনশিপ গল্প

` রুবির` সাজানো সংসার , স্বামী মারা যাবার পর  ছেলেদের আগলে রেখেছেন । তিন ছেলে এখন  কম্পিউটার ইঞ্জিনিয়ার । বড় ছেলে `হাবলু` P1 এ   সিনিয়র সফটওয়্যার ডেভেলপার  , মেজ `বাবলু` জুমশেপার এ ওয়েব ডেভেলপার এবং ছোট ছেলে `সালাম` গ্রাফিক্স ডিজাইনার ।

### ওয়ান টু ওয়ান  রিলেশন
গল্পে আপনি পড়ছেন `হাবলু` ,`বাবলু` , `সালাম` এর মা রুবি ।  তিন জন ছেলে  `রুবি` কে মা বলে সম্বোধন করে । সবার মা একজন এবং একটা মানুষ । মা বলে সম্বোধন করে শুধুমাত্র রুবি কে  , `ছেলের` সাথে  `মা`  সম্পর্ক  ওয়ান টূ ওয়ান  রিলেশন ।

### ওয়ান টু মেনি রিলেশন

রুবির তিন ছেলে এটা জানা আছে, `রুবি` একজন মানুষ হয়ে `হাবলু` ,`বাবলু` , `সালাম` তিন জনকে ছেলে বলে সম্বোধন করে , করবে এটাই চিরন্তন এটাই সত্য ,এটাই বাস্তব ।  `মায়ের`  সাথে  `ছেলে` সম্পর্ক টা হচ্ছে ওয়ান টু মেনি রিলেশন


### মেনি টু মেনি রিলেশন
`হাবলু` ,`বাবলু` , `সালাম` এদের মধ্যে সম্পর্ক ভাই , এরা একে  অন্যের ভাই ।  এটাই  মেনি টু মেনি রিলেশন ।



## Has Many Through
এটা এক ধরনের শর্টকাট এ  রিলেশন করার মতো  এবং অনেক গুলো  রিলেশন একবারে নিক্ষেপ করা । আপনি মানব বন্ধন দেখেছেন , সেখানে এক জনের হাতের সাথে অন্য আরেক জনের হাত যুক্ত হয়ে বিশাল লাইন হয় ।  প্রথম আর শেষের মানুষ এর সংযোগ আছে না ?

প্রথম আর শেষের মানুষের হাত সংযুক্ত না , তারপর ও কিন্তু যুক্ত আছে । কেমনে যুক্ত , আক জনের সাথে অন্য জনের হাত সংযুক্ত হয়ে সব শেষের মানুষ সংযোগ হয়েছে ।


* যেহেতু গল্পে গল্পে লারাভেল শিখব ,   একটা ছোট্ট গল্প
`পায়েল`  আর `কেয়া` দুই বোন । এইতো গত সপ্তাহে `কেয়ার`  বিয়ে  হয়েছে  `জাহিদ` এর সাথে । এতে  `কেয়া` আর `জাহিদ` এর একটা `রিলেশন` হইছে । আচ্ছা বলেন তো , `পায়েল` `জাহিদ` এর কে? ?

`পায়েল` আর `জাহিদ` এর অটোমেটিক একটা `রিলেশন` হয়ে গেছে ।  সেটা হয়েছে `কেয়ার` জন্য  ।

 <p class="tip"> এই দুই গল্পে  রিলেশনশিপ দেখানো হয়েছে  , মজার ব্যাপার হল এখানে [Foreign Key]() এর কথা   তুলে   ধরা  হয়েছে ।  </p>


``` html
countries
    id - integer
    name - string

users
    id - integer
    country_id - integer
    name - string

posts
    id - integer
    user_id - integer
    title - string
```
 <p class="tip">country_id  । user_id এসব [Foreign Key]() দিয়ে  করা । [Foreign Key]()  টা  এক  নজর দেখে নিতে পারেন </p>

 এখন Has Many Through প্রয়োগ এর পালা , `Country` [Model]() এ `hasManyThrough` এর মাধ্যমে  Post অ্যান্ড User >> [Model]() এর রিলেশন করা হয়েছে ।

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Country extends Model
{
    /**
     * Get all of the posts for the country.
     */
    public function posts()
    {
        return $this->hasManyThrough('App\Post', 'App\User');
    }
}
```

এখন রিলেশন কেমন মজুবত যাচাই করতে হবে । যাচাই করার জন্য [tinker]() এর ব্যবহার করেন , টার্মিনাল এ হিট করেন
` php artisan tinker `

```php
App\Country::find(1)->posts
```

## পলিমোরফিক রিলেশনস

 `পলিমোরফিক` রিলেশনস হচ্ছে `বহুরুপী` সম্পর্ক ।। আপনার আশেপাশে অনেক মানুষ আছে যারা বহু রুপী ।  আচ্ছা একটা গল্প বলি - সামিদ এবং রাহুল খুব ঘনিষ্ঠ বন্ধু । কোন কারনে তাদের মধ্যে বিরোধ দেখা দিয়েছে । ইমরান তাদের বন্ধু , কিন্তু ইমরান একটু হিংসুটে , সে তাদের এই বিরোধের সময় তা কাজে লাগাইছে । ইমরান সামিদ কে বলল দোস্ত রাহুল তো তোরে পচানি দিতাছে । আবার এই ইমরান  ই রাহুল এর কাছে গিয়ে বলতাছে দোস্ত জানস সামিদ না আমাক তোর সাথে মিশতে না করছে । এতে করে সামিদ আর রাহুল এর বিরোধ টা আরও বেশি হল । এখানে ইমরান পলিমোরফিক বা বহু রুপী হিসেবে কাজ করলো ।


 আপনি অনেক সাইটে দেখবেন পোস্ট এবং ভিডিও পোস্ট  এর জন্য আলাদা আলাদা সিঙ্গেল পেজ ব্যবহার করা হয় ।  পোস্ট এবং ভিডিও পোস্ট দুই পেজে আপনি কমেন্ট করতে পারেন । আচ্ছা দুইটা আলাদা পেজের জন্য কি দুটা আলাদা কমেন্ট টেবিল বানাবেন ?? যেমন `post_comments_table`  , `video_post_comments_table`

পলিমোরফিক রিলেশন দিয়ে শুধুমাত্র একটা টেবিল দিয়ে দুই পেজেই কমেন্ট কন্ট্রোল করতে পারবেন । আচ্ছা দেখুন কিভাবে পলিমোরফিক রিলেশন সেট করবেন -- `posts`  `videos`  `comments` আপনার তিনটা টেবিল ।  `comments` টেবিল পলিমোরফিক বা বহুরূপী হিসেবে সেট করবেন ।

```html
posts
    id - integer
    title - string
    body - text

videos
    id - integer
    title - string
    url - string

comments
    id - integer
    body - text
    commentable_id - integer
    commentable_type - string
```
`commentable_id` এখানে সেভ হবে `posts/videos id` এবং `commentable_type`  এ  থাকবে    টাইপ  `post` অথবা  `video` ।

এখন  [Model]() কেমন হবে দেখুন

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * Get all of the owning commentable models.
     */
    public function commentable()
    {
        return $this->morphTo();
    }
}

class Post extends Model
{
    /**
     * Get all of the post's comments.
     */
    public function comments()
    {
        return $this->morphMany('App\Comment', 'commentable');
    }
}

class Video extends Model
{
    /**
     * Get all of the video's comments.
     */
    public function comments()
    {
        return $this->morphMany('App\Comment', 'commentable');
    }
}
```
সব সেট আপ করা হয়ে গেছে  , একটু কাজ বাকি আছে  `Comment` [Model]() এ অ্যাড করে নিবেন ।

```php
use Illuminate\Database\Eloquent\Relations\Relation;

Relation::morphMap([
    'post' => 'App\Post',
    'video' => 'App\Video',
]);

```
  <p class="tip"> `'post' => 'App\Post'`  এখানে  ` 'টাইপ'  => 'App\Post'`  </p>

  এবার  দেখার পালা রিলেশন তা কেমন হল

```php
// post id 1 comments
  $post = App\Post::find(1);

foreach ($post->comments as $comment) {
    //
}
```

```php
// video id 1 comments
$post = App\Video::find(1);

foreach ($post->comments as $comment) {
    //
}
```



### মেনি টু মেনি  পলিমোরফিক

আপনি পলিমোরফিক সেকশন এ একটা গল্প পরেছেন , সামিদ এবং রাহুল এর সেখানে পলিমোরফিক ( বহুরূপী ) হিসেবে ছিল ইমরান । এটা তো বুঝতে পেরেছেন বহুরূপী হিসেবে প্রকাশ করতে হইলে  সর্ব নিম্ন দুজন মানুষ লাগে । একজন এর সাথে তো বহুরূপীতার পরিচয়  দেওয়া যাবে না ।  দুই এর অধিক এর সাথে পলিমোরফিক দেখানো টা হচ্ছে  মেনি টু মেনি  পলিমোরফিক।

  এখানে ৪ তা টেবিল `posts` `videos` `tags` `taggables` । ।

```php
posts
    id - integer
    name - string

videos
    id - integer
    name - string

tags
    id - integer
    name - string

taggables
    tag_id - integer
    taggable_id - integer
    taggable_type - string

```

  <p class="tip">  `tag_id`  টা হচ্ছে tags id , `taggable_id` হচ্ছে  posts/videos id ,  `taggable_type`  হচ্ছে   টাইপ  video/post  </p >

  * Model Structure - মডেল বিন্যাসঃ

  `morphToMany` মাধ্যমে মেনি টু মেনি  পলিমোরফিক প্রকাশ করা হয় ।

```php

  <?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * Get all of the tags for the post.
     */
    public function tags()
    {
        return $this->morphToMany('App\Tag', 'taggable');
    }
}
```


```php

  <?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Video extends Model
{
    /**
     * Get all of the tags for the post.
     */
    public function tags()
    {
        return $this->morphToMany('App\Tag', 'taggable');
    }
}
```


```php

<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    /**
     * Get all of the posts that are assigned this tag.
     */
    public function posts()
    {
        return $this->morphedByMany('App\Post', 'taggable');
    }

    /**
     * Get all of the videos that are assigned this tag.
     */
    public function videos()
    {
        return $this->morphedByMany('App\Video', 'taggable');
    }
}
```


```php
$post = App\Post::find(1);

foreach ($post->tags as $tag) {
    //
}
```
আপনি পোস্ট ১ এর সকল ট্যাগ লিস্ট পাবেন ।

```php
$post = App\Video::find(1);

foreach ($post->tags as $tag) {
    //
}
```
 অনুরূপ ভাবে আপনি  ভিডিও পোস্ট  ১ এর সকল ট্যাগ লিস্ট পাবেন ।


 * আপনি চাচ্ছেন কোন একটা ট্যাগ এ কতোগুলো পোস্ট আছে ।

```php

$tag = App\Tag::find(1);

foreach ($tag->videos as $video) {
    //
}
```

```php

$tag = App\Tag::find(1);

foreach ($tag->posts as $post) {
    //
}
```
Polymorphic Relations
The above syntax means the presence of the `active` class will be determined by the [truthiness](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) of the data property `isActive`.

You can have multiple classes toggled by having more fields in the object. In addition, the `v-bind:class` directive can also co-exist with the plain `class` attribute. So given the following template:

``` html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

And the following data:

``` js
data: {
  isActive: true,
  hasError: false
}
```

It will render:

``` html
<div class="static active"></div>
```

When `isActive` or `hasError` changes, the class list will be updated accordingly. For example, if `hasError` becomes `true`, the class list will become `"static active text-danger"`.

The bound object doesn't have to be inline:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

This will render the same result. We can also bind to a [computed property](computed.html) that returns an object. This is a common and powerful pattern:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal',
    }
  }
}
```

### Array Syntax

We can pass an array to `v-bind:class` to apply a list of classes:

``` html
<div v-bind:class="[activeClass, errorClass]">
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Which will render:

``` html
<div class="active text-danger"></div>
```

If you would like to also toggle a class in the list conditionally, you can do it with a ternary expression:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
```

This will always apply `errorClass`, but will only apply `activeClass` when `isActive` is `true`.

However, this can be a bit verbose if you have multiple conditional classes. That's why it's also possible to use the object syntax inside array syntax:

``` html
<div v-bind:class="[{ active: isActive }, errorClass]">
```

### With Components

> This section assumes knowledge of [Vue Components](components.html). Feel free to skip it and come back later.

When you use the `class` attribute on a custom component, those classes will be added to the component's root element. Existing classes on this element will not be overwritten.

For example, if you declare this component:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Then add some classes when using it:

``` html
<my-component class="baz boo"></my-component>
```

The rendered HTML will be:

``` html
<p class="foo bar baz boo">Hi</p>
```

The same is true for class bindings:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

When `isActive` is truthy, the rendered HTML will be:

``` html
<p class="foo bar active">Hi</p>
```

## Binding Inline Styles

### Object Syntax

The object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case (use quotes with kebab-case) for the CSS property names:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

It is often a good idea to bind to a style object directly so that the template is cleaner:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Again, the object syntax is often used in conjunction with computed properties that return objects.

### Array Syntax

The array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:

``` html
<div v-bind:style="[baseStyles, overridingStyles]">
```

### Auto-prefixing

When you use a CSS property that requires [vendor prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) in `v-bind:style`, for example `transform`, Vue will automatically detect and add appropriate prefixes to the applied styles.
