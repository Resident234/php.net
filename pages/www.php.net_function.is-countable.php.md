# is_countable



If you are unable to upgrade to PHP 7.3 (not released at the time of writing), you can use this simple polyfill:<br><br>

```
<?php
if (!function_exists('is_countable')) {
    function is_countable($var) {
        return (is_array($var) || $var instanceof Countable);
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-countable.php)

**[To root](/README.md)**