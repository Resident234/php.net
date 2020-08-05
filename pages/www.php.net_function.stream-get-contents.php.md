# stream_get_contents



It is important to know that stream_get_contents behaves differently with different versions of PHP. Consider the following<br><br>

```
<?php

$handle = fopen('file', 'w+'); // truncate + attempt to create
fwrite($handle, '12345'); // file position > 0
rewind($handle); // position = 0
$content = stream_get_contents($handle); // file position = 0 in PHP 5.1.6, file position > 0 in PHP 5.2.17!
fwrite($handle, '6789');
fclose($handle);

/**
 *
 * 'file' content
 * 
 * PHP 5.1.6:
 * 67895
 *
 * PHP 5.2.17:
 * 123456789
 *
 */
?>
```
<br><br>As a result, stream_get_contents() affects file position in 5.1, and do not affect file position in 5.2 or better.  

---

[Official documentation page](https://www.php.net/manual/en/function.stream-get-contents.php)

**[To root](/README.md)**