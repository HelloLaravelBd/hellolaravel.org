---
title: প্রধান কাঠামো বিন্যাস
type: হ্যালো লারাভেল ৫ । ৪
order: 3
---
---

হ্যালো লারাভেল !

##   কাঠামো বিন্যাসঃ


|  ক্রমিক নম্বর | ডিরেক্টরি নাম  |  বর্ণনা |
|--:|---|---|
| ১  |  app |   |
| ২  | bootstrap   |   |
|৩  |  Config  |   |
|৪   |  database |   |
|৫  |   public |   |
|৬  | resources  |   |
|৭  |  routes |   |
|৮ | storage  |   |
|৯| test  |   |
| ১০ | vendor  |   |
| ১১ | .env|   |
| ১২ | composer.json  | ম  |

### vendor
 ভেন্ডার সাধারণ অর্থ হচ্ছে লেনদেন । সব লারাভেল প্রোজেক্ট এর প্যাকেজ রাখার রুম। আপনার সব পাকেজ আকানে থাকে
 <p class="tip">লারাভেল প্রোজেক্ট এর প্যাকেজ রুম</p>

### env
   এই ফাইল এর ভিতরে আপনার গোপনীয় পাসওয়ার্ড , এপিআই সেট করবেন , যা আপনাকে অন্যান্য সাইটের সাথে আপনার সাইট কে সমন্বয় করবে।
   আপনি কোন কিছু পোস্ট অথবা ডাটা রাখবেন যা আপনাকে ডাটাবেসে রাখতে হবে । এখানে মাইএস কিউ এল এর কথা তুলে ধরি,  মাইএস কিউ এল আপনার লারাভেল প্রোজেক্ট এর সাথে  সেটআপ করে নিবেন ।

  ```html
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=ডাটাবেস নাম
DB_USERNAME= ইউজার নাম
DB_PASSWORD= যদি সেট ব্যবহার করেন // নাল
PASSWORD=
    ```
 <p class="tip">আপনার সাইটের সিন্দুক ঘর এই  .env ফাইল   </p>


### composer.json
 নতুন প্যাকেজ লারাভেল প্রোজেক্ট এ ব্যবহার করতে চাইলে composer.json এ রেজিস্টার করতে হবে ।
 নিচের কোড টা লক্ষ্য করুনঃ




 ``` js
 // composer.json
 "require": {
     "php": ">=5.6.4",
     "algolia/algoliasearch-client-php": "^1.17",
     "laravel/framework": "5.4.*",
     "laravel/scout": "^3.0",
     "laravel/tinker": "~1.0",
     "laravelcollective/html":"^5.2.0"
 },
 "require-dev": {
     "fzaninotto/faker": "~1.4",
     "mockery/mockery": "0.9.*",
     "phpunit/phpunit": "~5.7"
           },
 ```
 <p class="tip">`"laravelcollective/html":"^5.2.0"`  নামে নতুন একটা প্যাকেজ ব্যবহার করবেন , তাই প্রথমে প্যাকেজটি রেজিস্টার করে নিবেন । তারপর টার্মিনাল এ  `composer update` command করবেন [vendor]()  গিয়ে দেখবেন  `laravelcollective` নামে একটা ফোল্ডার তৈরি হয়েছে।  </p>
