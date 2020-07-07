# clearstatcache





unlink() does not clear the cache if you are performing file_exists() on a remote file like:





```
<?php

if (file_exists(&quot;ftp://ftp.example.com/somefile&quot;))

?>
```




In this case, even after you unlink() successfully, you must call clearstatcache().





```
<?php

unlink(&quot;ftp://ftp.example.com/somefile&quot;);

clearstatcache();

?>
```




file_exists() then properly returns false.

  

#

[Official documentation page](https://www.php.net/manual/en/function.clearstatcache.php)

**[To root](/README.md)**