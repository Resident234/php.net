# Using namespaces: fallback to global function/constant



You can use the fallback policy to provide mocks for built-in functions like time(). You therefore have to call those functions unqualified:<br><br>

```
<?php
namespace foo;

function time() {
    return 1234;
}

assert (1234 == time());
?>
```


However there's a restriction that you have to define the mock function before the first usage in the tested class method. This is documented in Bug #68541.

You can find the mock library ?>
```
mock at GitHub.  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.fallback.php)

**[To root](/README.md)**