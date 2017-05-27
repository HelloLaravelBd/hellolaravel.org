---
title: Alternative shouldReceive Syntax
type: Mockery/Mockery
order: 1
---
---
As of Mockery 1.0.0, we support calling methods as we would call any PHP method,
and not as string arguments to Mockery ``should*`` methods.

The two Mockery methods that enable this are ``allows()`` and ``expects()``.

Allows
------

We use ``allows()`` when we create stubs for methods that return a predefined
return value, but for these method stubs we don't care how many times, or if at
all, were they called.

```php
    $mock = \Mockery::mock('MyClass');
    $mock->allows([
        'name_of_method_1' => ['1st call return value', '2nd call return value'],
        'name_of_method_2' => ['1st call return value', '2nd call return value'],
    ]);
```
This is equivalent with the following ``shouldReceive`` syntax:

```php
    $mock = \Mockery::mock('MyClass');
    $mock->shouldReceive([
        'name_of_method_1' => ['1st call return value', '2nd call return value'],
        'name_of_method_2' => ['1st call return value', '2nd call return value'],
    ]);
```
Note that with this format, we also tell Mockery that we don't care about the
arguments to the stubbed methods.

If we do care about the arguments, we would do it like so:

```php
    $mock = \Mockery::mock('MyClass');
    $mock->allows()
        ->name_of_method_1($arg1)
        ->andReturn(['1st call return value', '2nd call return value']);
```
This is equivalent with the following ``shouldReceive`` syntax:

```php
    $mock = \Mockery::mock('MyClass');
    $mock->shouldReceive('name_of_method_1')
        ->with($arg1)
        ->andReturn(['1st call return value', '2nd call return value']);
```
Expects
-------

We use ``expects()`` when we want to verify that a particular method was called:

```php
    $mock = \Mockery::mock('MyClass');
    $mock->expects()
        ->name_of_method_1($arg1)
        ->andReturn('return value');
```
This is equivalent with the following ``shouldReceive`` syntax:

```php
    $mock = \Mockery::mock('MyClass');
    $mock->shouldReceive('name_of_method_1')
        ->once()
        ->with($arg1)
        ->andReturn('return value');
```
By default ``expects()`` sets up an expectation that the method should be called
once and once only. If we expect more than one call to the method, we can change
that expectation:


```php
    $mock = \Mockery::mock('MyClass');
    $mock->expects()
        ->name_of_method_1($arg1)
        ->twice()
        ->andReturn('return value');
```
