# PHP Nuggets

## 1. Always use strict checking unless you want to intentionally use type juggling

### Examples of Type Juggling in PHP 7.4

PHP 7.4 uses type juggling, which means it automatically converts variables to the needed type during operations. 

For example, when you add two values or compare them with ==, PHP converts one or both operands to compatible types

⚠️ When converting strings to integers, PHP grabs numeric characters from the start and ignores the rest:
```
"10hello" == 10; // true, because "10hello" converts to 10
```

☝️ This behavior can be exploited to bypass security checks by manipulating input values.

### Boolean Comparisons and Type Juggling
```
0 == "" is true because both are falsy.
0 == "hello" is also true because "hello" converts to 0 when cast to integer.
```

### PHP 8 Changes
PHP 8 changes type juggling behavior to be safer:

When comparing integer and string with ==, PHP converts the integer to a string instead of the other way around:
```
10 == "10"; // true
10 == "10hello"; // false in PHP 8
```

☝️This prevents attacks that rely on partial numeric string conversion.

However, boolean comparisons with loose equality still behave similarly:
```
true == 1; // true
true == "anything"; // true
```

### Summary
So in summary, always use strict comparison (===) to avoid type juggling:
```
10 === "10"; // false
```

Always prefer strict comparisons unless you explicitly want loose comparison behavior which enables **type juggling**.

### Reference
- [Laracasts > Laravel Security Through Examples > Type Juggling](https://laracasts.com/series/laravel-security-through-examples/episodes/10) 

## 2. Using `hash_equals()`

(PHP 5 >= 5.6.0, PHP 7, PHP 8)

hash_equals — Timing attack safe string comparison.

This checks whether two strings are equal without leaking information about the contents of known_string via the execution time. 

This function can be used to mitigate timing attacks. Performing a regular comparison with === will take more or less time to execute depending on whether the two values are different or not and at which position the first difference can be found, thus leaking information about the contents of the secret known_string. 

Example:
```
<?php
$secretKey = '8uRhAeH89naXfFXKGOEj';

// Value and signature are provided by the user, e.g. within the URL
// and retrieved using $_GET.
$value = 'username=rasmuslerdorf';
$signature = '8c35009d3b50caf7f5d2c1e031842e6b7823a1bb781d33c5237cd27b57b5f327';

if (hash_equals(hash_hmac('sha256', $value, $secretKey), $signature)) {
    echo "The value is correctly signed.", PHP_EOL;
} else {
    echo "The value was tampered with.", PHP_EOL;
}
```

### References
- [Laracasts > Laravel Security Through Examples > Type Juggling](https://laracasts.com/series/laravel-security-through-examples/episodes/10)
- [php.net > hash_equals](https://www.php.net/manual/en/function.hash-equals.php)


