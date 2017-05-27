---
title: Eloquent Model
type:  হ্যালো লারাভেল ৫ । ৪
order: 11
---

## মডেল  

মডেল ডাটাবেস এর সাথে যুক্ত । [এম  ভি সি  ]() তে আলোচনা করা হয়েছে , এক নজর দেখে নিতে পারেন টপিক টা ।

* মডেল বানাতে কমান্ড করতে হবে

```php
php artisan make:model Flight
```

* মডেল বানানোর সময় ডাটা টেবিল বানাতে চাইলে  কমান্ড করতে হবে

```php
php artisan make:model Flight --migration

php artisan make:model Flight -m
```
<p class="tip">মনে রাখবেন মডেল নাম সব সময় ক্যাপিটাল লেটার (বড় হাতের) দিয়ে শুরু করতে হবে.</p>

* App ডিরেক্টরি  তে গেলে  Flight.php নামে ফাইল তৈরি হবে , এটাই আপনার Flight মডেল । মডেল বাসিক গঠন -

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    //
}
```



```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The table associated with the model.
     *
     * @var string
     */
    protected $table = 'my_flights';
}
```
মডেল কে কিভাবে কন্ট্রোলার এ যুক্ত করবেন ?

ডাটাবেস এ দেখানো হয়েছে ডাটাবেস হতে ডাটা কিভাবে পাবেন । সেখানে দুইটা পদ্ধতি দেখানো হয়েছে একটা সরাসরি টেবিল নাম  দিয়ে আর একটা মডেল দিয়ে ।
