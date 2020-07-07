# include_once





In response to what a user wrote 8 years ago regarding include_once being ran two times in a row on a non-existent file:

Perhaps 8 years ago that was the case, however I have tested in PHP 5.6, and I get this:

$result = include_once &apos;fakefile.php&apos;;&#xA0; // $result = false
$result = include_once &apos;fakefile.php&apos;&#xA0;&#xA0; // $result is still false

  

#



If you include a file that does not exist with include_once, the return result will be false. 

If you try to include that same file again with include_once the return value will be true.

Example:


```
<?php
var_dump(include_once &apos;fakefile.ext&apos;); // bool(false)
var_dump(include_once &apos;fakefile.ext&apos;); // bool(true)
?>
```


This is because according to php the file was already included once (even though it does not exist).

  

#

[Official documentation page](https://www.php.net/manual/en/function.include-once.php)

**[To root](/README.md)**