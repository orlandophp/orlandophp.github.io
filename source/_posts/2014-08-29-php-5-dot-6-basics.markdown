---
layout: post
title: "PHP 5.6 Basics"
date: 2014-08-29 08:42:18 -0400
comments: true
categories: [PHP, 5.6.0, Features]
published: true
---

PHP 5.6.0 was released yesterday, which marks another milestone for PHP development. With it comes a nice set of new features and a few BC breaks. I'll go over some of the more prominent points and link to a list of everything for your deeper perusal. As for features, the most interesting to me are _improved handling of variatic functions_ and _argument unpacking_ with `...`, _constant scalar expressions_, _exponentiation_, and file downloads that can exceed _2 GB_. There are a few more, but these are, perhaps, the ones that the average developer may encounter/use on a regular basis.

<!--more-->

So, lets get down to some examples.

***

### New Features

#### Improved handling of variadic functions and Argument unpacking with `...`

Variadic fuctions and argument unpacking are perhaps best explained through example.

We can indicate, in a function's parameter, that we are willing to accept any number of arguments by using the `...` (elipsis), which eliminates our need to rely on func_get_args().

``` php Variadic Function Example
function add_these(...$numbers) {
    echo array_sum($numbers);
}
add_these(1, 2, 3, 4, 5); // Will return 15
```

_The `add_these` function will take any number of arguments we throw at it and within our function that `$numbers` variable becomes an array that can be passed to other functions or iterated through._

The variatic function's parameter can also be type-hinted.

``` php Variadic Function Example with Type Hinting
function display_users(User ...$users) {
    foreach ($users as $user) {
        echo $user->name . "\n";
        echo $user->email . "\n\n";
    }
}
display_users($users);
```

_Given that `$users` is 2 User objects with a name and email property, we should see the following output._

``` bash Example Output
    John Doe
    john.doe@doezer.org

    Jane Doe
    jane.doe@doezer.org

```

Unpacking works similarly,

``` php Array Unpacking with '...'
function get_element($index, $array) {
    echo $array[$index];
}

$index = 4;
$numbers = [1, 2, 3, 4, 5];
$input = [$index, $numbers];
get_element(...$input); // displays '5'
```

_So, passing an array into the function with a preceding `...` will use each index in the array as a different argument for the function._

Ok, a little take home for you. Consider this outout :

``` php Unpacking Homework
function get_element($index, ...$array) {
    echo $array[$index];
}

$input = [1, 2, 3, 4, 5];
get_element(...$input);
```

#### Constant Scalar Expressions

Typically, we're used to constants being static values:

``` php Typical, Pre-PHP 5.6 const Syntax
const THIS_IS_IT = 'It';
```

In PHP 5.6, you'll be able to provide an expression for constant values:

``` php New const Syntax with Scalar Expressions
const THIS_THING = 5 + 25;
const THIS_IS_NOT_IT = THIS_THING . " is not It";

echo THIS_IS_NOT_IT; // 30 is not It
```

#### Exponentiation

This feature is an addition to the math operators that basically provides a shortcut (and more readable text) for working with `pow()`. Essentially, if you've used `pow(4, 5)`, you could now use `4**5` instead. All pretty straight forward.

For example:

##### Previously:

``` php Using 'pow()' for Exponentiation
$e = pow(2, 3);
echo $e; // 8
```

##### Now:

``` php New, shorter Syntax for Exponentiation
$e = 2**3;
echo $e; // 8
```

_`**` ends up a little more concise and seems easier to scan over (to me) than the `pow()` function._

#### File downloads exceeding 2 GB

With larger and more complex systems, higher resolution photos, and larger documents comes the need for larger uploads/downloads. Thank the PHP Lords for this one!

#### Other goodies

As for other goodies, go check out the [PHP 5.6](http://php.net/manual/en/migration56.new-features.php) page for details and examples of the other features, including _GMP operator overloading_, the new `hash_equals()` function, bundled _phpdbg_, and the *new* magic method `__debugInfo()`, to name a few. Although this is a minor update, and there only seem to be a few changes, the changes that are being made with PHP are heralding the future of PHP.

***

### Backward Compatability

As for backward compatability, there are a few things that have be broken.

- For starters, Windows 2003 and XP are no longer supported (not too shocking).
- `self`, `parent`, and `static` are now __always__ (lower) case sensitive.
- The PHP logo GUIDs have been removed, effecting the following functions:

``` php Removed GUID Logo Functions
    php_logo_guid();
    php_egg_logo_guid();
    php_real_logo_guid();
    zend_logo_guid();
```

- Plus, a few [others](http://php.net/manual/en/migration55.incompatible.php)

---

Pretty interesting changes so far, perhaps we'll still see one more minor version before PHP 7.

We'd love to hear your comments below, so feel free to correct me, ask questions, or just rant!

Enjoy your weekend!

Patrick
