---
title: নেমস্পেস - Namespace
type:  হ্যালো লারাভেল ৫ । ৪
order: 30
---

---

## নেমস্পেস কি?

নেমস্পেস মুলত ডিরেক্টরি / ফাইল নির্দেশক ।

### নেমস্পেস সম্পর্কে বিস্তারিত

নেমস্পেস সম্পর্কে একটা গল্প শোনেন - রবি আর সুজা দুই ভাই , তারা দুজনে একই ক্লাসে পরে । তাদের মাঝে খুব ঝগড়া হয় , বিশেষ করে কলম নিয়ে বেশি । কারন তারা তাদের পরার জিনিস পত্র একই ডেস্কের ড্রয়ারে রাখে । কলম একই কালার এর হওয়ার জন্য কে কার কলম ববহার করছে বুঝতে পারে না ।  এই সমস্যা সমাধানের জন্য রবির বাবা তাদের কলম এ লেবেল লাগিয়ে দেয় । এতে আর সমস্যা হয় না ।  

আপানার লারাভেল প্যাকেজে বা অন্য যেকোনো পি এইচ পি ডেভেলপমেন্ট প্যাকেজে যদি একই নামে একাধিক `function` ,  `class` , `interface` , `constant` , `trait` থাকে তখন আপনি সমস্যায় পরবেন । কোন তা কল করতাছেন সেটা ফিক্স করা সম্ভব না । এই সমস্যা সমাধানের জন্য নেমস্পেস ব্যবহার করা হয় । নেমস্পেস তা লেবেল বা স্তিকার  এর মত কাজ করে ।

নেমস্পেস কে প্রকাশ করা  হয় `namespace` দিয়ে  । লারাভেল এর সকল মডেল বা কন্ট্রোলার এর সকল ফাইল এর টপ এ
namespace App;   বা  এমন `namespace App\Http\Controllers;` সাব-নেমস্পেস ব্যাকস্লাশ (`\`)  দিয়ে পৃথক করা হয় ।

laravel এর একটি ফাইল  একটা নেমস্পেস হবে । বাকি গুলা use ব্যবহার করে নেমস্পেশ দেখাতে হবে ।


### নেমস্পেস  ইমপোর্ট (importing/aliasing)

নেমস্পেসের মাধ্যমে ইমপোর্ট করা হয় `use` operator দিয়ে । লারাভেল প্যাকেজ এর ডিফল্ট Controller এর কোড ।


```php
<?php

namespace App\Http\Controllers;

use Illuminate\Foundation\Bus\DispatchesJobs;
use Illuminate\Routing\Controller as BaseController;
use Illuminate\Foundation\Validation\ValidatesRequests;
use Illuminate\Foundation\Auth\Access\AuthorizesRequests;

class Controller extends BaseController
{
    use AuthorizesRequests, DispatchesJobs, ValidatesRequests;
}
```
* `aliasing / উপনাম/ডাক-নাম` - সবার নাম অনেক বড় হয় , কিন্তু সহজে ডাকার জন্য ছোট একটা নাম দেওয়া হয় , জাকে ডাক নাম বলা হয় । এই `ছোট বা ডাক নাম` কে `alias` বলা হয় ।

```php
use Illuminate\Routing\Controller as BaseController;
```

`as` দিয়ে  `aliasing` নেমস্পেস ইমপোর্ট করা হয় । `Illuminate\Routing\Controller` এর ডাক-নাম `BaseController` ।
তাই Illuminate\Routing\Controller এর পরিবর্তে `BaseController` নামে extends(`class Controller extends BaseController`) করে `Illuminate\Routing\Controller` এর সব কিছু চাইল্ড পেয়েছে ।  

- [Forum](http://forum.vuejs.org/): The best place to ask questions and get answers about Vue and its ecosystem.
- [Gitter Channel](https://gitter.im/vuejs/vue): A place for devs to meet and chat. You can ask questions here too, but the forum is the better platform, since the discussions are threaded.
- [Github](https://github.com/vuejs): If you have a bug to report or feature to request, that's what the GitHub issues are for. We also welcome pull requests!

### extends , implements , traits

অবজেক্ট ওরিয়েন্ট প্রোগ্রামিং ল্যাঙ্গুয়েজ এ extends , implements পড়েছেন । পি এইচ পি তে স্পেশাল traits । এই তিনটা টপিক জানা না থাকলে দেখে নিবেন ।

অন্যান্য ল্যাঙ্গুয়েজে  মাল্টিপল ইনহেরিটান্স সম্ভব কিন্তু পি এইচ পি তে  মাল্টিপল ইনহেরিটান্স সাপোর্ট করে না  । মাল্টিলেভেল  ইনহেরিটান্স  সাপোর্ট করে । মাল্টিপল ইনহেরিটান্স এর কাজ traits দিয়ে করতে পারবেন ।

```php
<?php
trait Hello {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait World {
    public function sayWorld() {
        echo 'World';
    }
}

class MyHelloWorld {
    use Hello, World;
    public function sayExclamationMark() {
        echo '!';
    }
}

$o = new MyHelloWorld();
$o->sayHello();
$o->sayWorld();
$o->sayExclamationMark();
?>
```
