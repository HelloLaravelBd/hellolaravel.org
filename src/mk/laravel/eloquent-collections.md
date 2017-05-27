---
title: Eloquent Collections
type: laravel 5.4
order: 10.3
---

<a name="introduction"></a>
## Introduction

All multi-result sets returned by Eloquent are instances of the `Illuminate\Database\Eloquent\Collection` object, including results retrieved via the `get` method or accessed via a relationship. The Eloquent collection object extends the Laravel [base collection](/mk/laravel/collections.html), so it naturally inherits dozens of methods used to fluently work with the underlying array of Eloquent models.

Of course, all collections also serve as iterators, allowing you to loop over them as if they were simple PHP arrays:
```php
    $users = App\User::where('active', 1)->get();

    foreach ($users as $user) {
        echo $user->name;
    }
```
However, collections are much more powerful than arrays and expose a variety of map / reduce operations that may be chained using an intuitive interface. For example, let's remove all inactive models and gather the first name for each remaining user:
```php
    $users = App\User::where('active', 1)->get();

    $names = $users->reject(function ($user) {
        return $user->active === false;
    })
    ->map(function ($user) {
        return $user->name;
    });
```
<p class="tip"> While most Eloquent collection methods return a new instance of an Eloquent collection, the `pluck`, `keys`, `zip`, `collapse`, `flatten` and `flip` methods return a [base collection](/mk/laravel/collections.html) instance. Likewise, if a `map` operation returns a collection that does not contain any Eloquent models, it will be automatically cast to a base collection.</p>

<a name="available-methods"></a>
## Available Methods

### The Base Collection

All Eloquent collections extend the base [Laravel collection](/mk/laravel/collections.html) object; therefore, they inherit all of the powerful methods provided by the base collection class:
```php
<style>
    #collection-method-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #collection-method-list a {
        display: block;
    }
</style>
```
<div id="collection-method-list" markdown="1">

[all](/mk/laravel/collections.html#method-all)
[average](/mk/laravel/collections.html#method-average)
[avg](/mk/laravel/collections.html#method-avg)
[chunk](/mk/laravel/collections.html#method-chunk)
[collapse](/mk/laravel/collections.html#method-collapse)
[combine](/mk/laravel/collections.html#method-combine)
[contains](/mk/laravel/collections.html#method-contains)
[containsStrict](/mk/laravel/collections.html#method-containsstrict)
[count](/mk/laravel/collections.html#method-count)
[diff](/mk/laravel/collections.html#method-diff)
[diffKeys](/mk/laravel/collections.html#method-diffkeys)
[each](/mk/laravel/collections.html#method-each)
[every](/mk/laravel/collections.html#method-every)
[except](/mk/laravel/collections.html#method-except)
[filter](/mk/laravel/collections.html#method-filter)
[first](/mk/laravel/collections.html#method-first)
[flatMap](/mk/laravel/collections.html#method-flatmap)
[flatten](/mk/laravel/collections.html#method-flatten)
[flip](/mk/laravel/collections.html#method-flip)
[forget](/mk/laravel/collections.html#method-forget)
[forPage](/mk/laravel/collections.html#method-forpage)
[get](/mk/laravel/collections.html#method-get)
[groupBy](/mk/laravel/collections.html#method-groupby)
[has](/mk/laravel/collections.html#method-has)
[implode](/mk/laravel/collections.html#method-implode)
[intersect](/mk/laravel/collections.html#method-intersect)
[isEmpty](/mk/laravel/collections.html#method-isempty)
[isNotEmpty](/mk/laravel/collections.html#method-isnotempty)
[keyBy](/mk/laravel/collections.html#method-keyby)
[keys](/mk/laravel/collections.html#method-keys)
[last](/mk/laravel/collections.html#method-last)
[map](/mk/laravel/collections.html#method-map)
[mapWithKeys](/mk/laravel/collections.html#method-mapwithkeys)
[max](/mk/laravel/collections.html#method-max)
[median](/mk/laravel/collections.html#method-median)
[merge](/mk/laravel/collections.html#method-merge)
[min](/mk/laravel/collections.html#method-min)
[mode](/mk/laravel/collections.html#method-mode)
[nth](/mk/laravel/collections.html#method-nth)
[only](/mk/laravel/collections.html#method-only)
[partition](/mk/laravel/collections.html#method-partition)
[pipe](/mk/laravel/collections.html#method-pipe)
[pluck](/mk/laravel/collections.html#method-pluck)
[pop](/mk/laravel/collections.html#method-pop)
[prepend](/mk/laravel/collections.html#method-prepend)
[pull](/mk/laravel/collections.html#method-pull)
[push](/mk/laravel/collections.html#method-push)
[put](/mk/laravel/collections.html#method-put)
[random](/mk/laravel/collections.html#method-random)
[reduce](/mk/laravel/collections.html#method-reduce)
[reject](/mk/laravel/collections.html#method-reject)
[reverse](/mk/laravel/collections.html#method-reverse)
[search](/mk/laravel/collections.html#method-search)
[shift](/mk/laravel/collections.html#method-shift)
[shuffle](/mk/laravel/collections.html#method-shuffle)
[slice](/mk/laravel/collections.html#method-slice)
[sort](/mk/laravel/collections.html#method-sort)
[sortBy](/mk/laravel/collections.html#method-sortby)
[sortByDesc](/mk/laravel/collections.html#method-sortbydesc)
[splice](/mk/laravel/collections.html#method-splice)
[split](/mk/laravel/collections.html#method-split)
[sum](/mk/laravel/collections.html#method-sum)
[take](/mk/laravel/collections.html#method-take)
[tap](/mk/laravel/collections.html#method-tap)
[toArray](/mk/laravel/collections.html#method-toarray)
[toJson](/mk/laravel/collections.html#method-tojson)
[transform](/mk/laravel/collections.html#method-transform)
[union](/mk/laravel/collections.html#method-union)
[unique](/mk/laravel/collections.html#method-unique)
[uniqueStrict](/mk/laravel/collections.html#method-uniquestrict)
[values](/mk/laravel/collections.html#method-values)
[when](/mk/laravel/collections.html#method-when)
[where](/mk/laravel/collections.html#method-where)
[whereStrict](/mk/laravel/collections.html#method-wherestrict)
[whereIn](/mk/laravel/collections.html#method-wherein)
[whereInStrict](/mk/laravel/collections.html#method-whereinstrict)
[whereNotIn](/mk/laravel/collections.html#method-wherenotin)
[whereNotInStrict](/mk/laravel/collections.html#method-wherenotinstrict)
[zip](/mk/laravel/collections.html#method-zip)

</div>

<a name="custom-collections"></a>
## Custom Collections

If you need to use a custom `Collection` object with your own extension methods, you may override the `newCollection` method on your model:

```php
    <?php

    namespace App;

    use App\CustomCollection;
    use Illuminate\Database\Eloquent\Model;

    class User extends Model
    {
        /**
         * Create a new Eloquent Collection instance.
         *
         * @param  array  $models
         * @return \Illuminate\Database\Eloquent\Collection
         */
        public function newCollection(array $models = [])
        {
            return new CustomCollection($models);
        }
    }
```
Once you have defined a `newCollection` method, you will receive an instance of your custom collection anytime Eloquent returns a `Collection` instance of that model. If you would like to use a custom collection for every model in your application, you should override the `newCollection` method on a base model class that is extended by all of your models.
