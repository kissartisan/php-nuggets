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



