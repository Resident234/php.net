# Other filters



Here is an example, since I cannot find one through out php.net"<br><br>

```
<?php

/**
 * Strip whitespace (or other non-printable characters) from the beginning and end of a string
 * @param string $value
 */
function trimString($value)
{
    return trim($value);
}

$loginname = filter_input(INPUT_POST, 'loginname', FILTER_CALLBACK, array('options' => 'trimString'));?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.misc.php)

**[To root](/README.md)**