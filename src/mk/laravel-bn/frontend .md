---
title:  থিম / ফ্রন্ট এণ্ড সেটআপ
type: হ্যালো লারাভেল ৫ । ৪
order: 4
---
---হ্যাঁলো লারাভেল !

---
আমরা যেকোনো সাইট ডীভীলোপ  করার আগে মনের মতো টেমপ্লেট বা থিম নির্বাচন করি । প্রতিটা থিম এর ফোল্ডার একই `css` ,` js` , `imges` and` html`


## Public folder
 * js
   * jquery.js
   * vue.js
 * css
   * main.css
   * login.css
 * images
   * logo.png
   * banner.jpg

আপনি পাবলিক এ যা রাখবেন , শুধু কল করলেই পেয়ে যাবেন ।

### Resources folder
Resources folder `assets` , `lang` , `views`

 এখন Resources views এর গল্প । আপনার সাইট এর ফ্রন্ট এর সকল পি এইচ পি ফাইল এখানে থাকবে । এখানে যে যার ইচ্ছে মত সাজিয়ে গুছিয়ে ফাইল গুলো রাখে ।

 একটা সাইট এ `header`, `footer` , `sidebar`  এসব সব পেজেই লাগে । আপনি কি এসব  বার বার প্রত্যেক  পেজে যুক্ত করবেন ? তাতে সময় অপচয় ।
 । তাই যেসব   কমন , সব পেজে লাগবে, সেসব  `partials` নামে ফোল্ডারে রাখবেন ।

 * partials

   *  _head.blade.php
   *  _aside.blade.php
   *  _footer.blade.php
   *  _mystyle.blade.php
   * _scripts.blade.php


এখন এসব ফাইল  @include  দিয়ে এড করতে হয়   । নিচের কোড তা দেখে নিন -

* `main.blade.php`

```html
<!DOCTYPE html>
<html>
<head>
  @include('partials\_head')
  @include('partials\_mystyle')

</head>
<body class="framed main-scrollable">
  <div class="wrapper">
      @include('partials\_nav')
      <div class="dashboard">
        @include('partials\_aside')

          <div class="main">
            @yield('maintheme')
          </div>

      </div>
  </div>


    @include('partials\_footer')
      @include('partials\_scripts')

</html>
```
কোডে  _mystyle , _scripts এসব আছে । আপনার সব ফাইল  _mystyle , ফাইল  _scripts এ রাখবেন । আর একটা জিনিস আপনি দেখে হইত ভাবছেন @yield টা কি । কোড তা দেখেন @yield('maintheme') তার মানে হচ্ছে সব পেজে আমরা শুধু main ক্লাস টা পরিবরতন করব । পরিবর্তন করার জন্য @yield ব্যবহার করা হয়ে থাকে ।


* _head.blade.php

```php
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Laravel Blog @yield('title')</title>
```

title টা সব সময় আলাদা আলাদা করবেন , সেজন্য এখানেও  @yield() ব্যবহার করবেন ।

*  _mystyle.blade.php

```php
<link href="css/demo.css" rel="stylesheet">
<link rel="stylesheet" href="css/custom.css">
@yield('styleme')
```

সব কাজের শেষে আপনি একটা ফ্রেম ফাইল বানিয়েছেন  `main.blade.php`  । এই ফাইল টা আপনার সব পেজ এ @extends('main') করবেন , তাহলে আপনার  main ফাইল সব পেজ পেয়ে যাবে । এখন @yield() নিয়ে কিছু কথা --

```php
@section('styleme')
  <link rel="stylesheet" href="js/datatable/dataTables.bootstrap.css">
  <link rel="stylesheet" href="js/datatable/dark.css">

@endsection
```
### Extending A Layout

```php
@extends('layouts.app')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
```

থিমের কাজ আপনার কমপ্লিট ।

## Condition / Resource
Condition - শর্ত । Condition ছাড়া আমরা কিছুই করি না । কোন শর্তে আপনি লারাভেল শিখছেন , হইত লারাভেল জনপ্রিয় তাই , বা ইনকাম ভাল বা আপনি শিখার আগ্রহ , যাই হক শর্ত একটা আছে । আপনি Condition সম্পর্কে জানেন , আকানে বিস্তারিত বলার কিছু নাই , না জেনে থাকলে শিখে নিবেন ।

`@if` , `@else`
```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif
```

`@unless`
```php
@unless (Auth::check())
    You are not signed in.
@endunless

```

`@isset` , `@empty`
```php
@isset($records)
    // $records is defined and is not null...
@endisset

@empty($records)
    // $records is "empty"...
@endempty
```

## Loop Master
আপনি অবশ্যই লুপের সাথে পরিচিত । সহজ ভাষাতে একই কাজ বার বার একই ভাবে করা হচ্ছে Loop । এখানে

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

আপনি লুপ শেষ / স্কিপ করতে চান -- `@break`, `@continue` ,
```php
@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{{ $user->name }}</li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach
```
* আপনি এভাবে ও একই ফলাফল পাবেন ।  যেকোনটা ফলো করতে পারেন ।
```php
@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{{ $user->name }}</li>

    @break($user->number == 5)
@endforeach
```
### The $loop Variable

|  নাম | বর্ণনা |
|---|---|
|  `$loop->index	` | আপনার লুপ ০ হতে দেখাবে  |
|  `$loop->iteration	` | আপনার লুপ ১ হতে দেখাবেে  |
| `$loop->remaining	`  | আপনার লুপে একই নামে কয়টা আইটেম আছে সেটার সংখ্যা দেখাবে  |
|  `$loop->count	` | আপনার লুপে কতগুলো আইটেম আছে তার সংখ্যা দেখাবে  |
|  `$loop->first	` |  আপনার লুপের প্রথম আইটেম  দেখাবে |
|  `$loop->last	` | আপনার লুপের শেষের আইটেম  দেখাবে   |
|  `$loop->depth	` | nested লুপে ববহার করা হয়ে থাকে  |
|  `$loop->parent	` | nested লুপে ববহার করা হয়ে থাকে  |


When you pass a plain JavaScript object to a Vue instance as its `data` option, Vue will walk through all of its properties and convert them to getter/setters using [Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty). This is an ES5-only and un-shimmable feature, which is why Vue doesn't support IE8 and below.

The getter/setters are invisible to the user, but under the hood they enable Vue to perform dependency-tracking and change-notification when properties are accessed or modified. One caveat is that browser consoles format getter/setters differently when converted data objects are logged, so you may want to install [vue-devtools](https://github.com/vuejs/vue-devtools) for a more inspection-friendly interface.

Every component instance has a corresponding **watcher** instance, which records any properties "touched" during the component's render as dependencies. Later on when a dependency's setter is triggered, it notifies the watcher, which in turn causes the component to re-render.

![Reactivity Cycle](/images/data.png)

## Change Detection Caveats

Due to the limitations of modern JavaScript (and the abandonment of `Object.observe`), Vue **cannot detect property addition or deletion**. Since Vue performs the getter/setter conversion process during instance initialization, a property must be present in the `data` object in order for Vue to convert it and make it reactive. For example:

``` js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` is now reactive

vm.b = 2
// `vm.b` is NOT reactive
```

Vue does not allow dynamically adding new root-level reactive properties to an already created instance. However, it's possible to add reactive properties to a nested object using the `Vue.set(object, key, value)` method:

``` js
Vue.set(vm.someObject, 'b', 2)
```

You can also use the `vm.$set` instance method, which is just an alias to the global `Vue.set`:

``` js
this.$set(this.someObject, 'b', 2)
```

Sometimes you may want to assign a number of properties to an existing object, for example using `Object.assign()` or `_.extend()`. However, new properties added to the object will not trigger changes. In such cases, create a fresh object with properties from both the original object and the mixin object:

``` js
// instead of `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

There are also a few array-related caveats, which were discussed earlier in the [list rendering section](list.html#Caveats).

## Declaring Reactive Properties

Since Vue doesn't allow dynamically adding root-level reactive properties, you have to initialize Vue instances by declaring all root-level reactive data properties upfront, even just with an empty value:

``` js
var vm = new Vue({
  data: {
    // declare message with an empty value
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// set `message` later
vm.message = 'Hello!'
```

If you don't declare `message` in the data option, Vue will warn you that the render function is trying to access a property that doesn't exist.

There are technical reasons behind this restriction - it eliminates a class of edge cases in the dependency tracking system, and also makes Vue instances play nicer with type checking systems. But there is also an important consideration in terms of code maintainability: the `data` object is like the schema for your component's state. Declaring all reactive properties upfront makes the component code easier to understand when revisited later or read by another developer.

## Async Update Queue

In case you haven't noticed yet, Vue performs DOM updates **asynchronously**. Whenever a data change is observed, it will open a queue and buffer all the data changes that happen in the same event loop. If the same watcher is triggered multiple times, it will be pushed into the queue only once. This buffered de-duplication is important in avoiding unnecessary calculations and DOM manipulations. Then, in the next event loop "tick", Vue flushes the queue and performs the actual (already de-duped) work. Internally Vue tries native `Promise.then` and `MutationObserver` for the asynchronous queuing and falls back to `setTimeout(fn, 0)`.

For example, when you set `vm.someData = 'new value'`, the component will not re-render immediately. It will update in the next "tick", when the queue is flushed. Most of the time we don't need to care about this, but it can be tricky when you want to do something that depends on the post-update DOM state. Although Vue.js generally encourages developers to think in a "data-driven" fashion and avoid touching the DOM directly, sometimes it might be necessary to get your hands dirty. In order to wait until Vue.js has finished updating the DOM after a data change, you can use `Vue.nextTick(callback)` immediately after the data is changed. The callback will be called after the DOM has been updated. For example:

``` html
<div id="example">{{ message }}</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // change data
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

There is also the `vm.$nextTick()` instance method, which is especially handy inside components, because it doesn't need global `Vue` and its callback's `this` context will be automatically bound to the current Vue instance:

``` js
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: 'not updated'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = 'updated'
      console.log(this.$el.textContent) // => 'not updated'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => 'updated'
      })
    }
  }
})
```
