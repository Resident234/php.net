# clearstatcache



unlink() does not clear the cache if you are performing file_exists() on a remote file like:<br><br>

```
<?php
if (file_exists("ftp://ftp.example.com/somefile"))
?>
```


In this case, even after you unlink() successfully, you must call clearstatcache().



```
<?php
unlink("ftp://ftp.example.com/somefile");
clearstatcache();
?>
```
<br><br>file_exists() then properly returns false.  

---

[Official documentation page](https://www.php.net/manual/en/function.clearstatcache.php)

**[To root](/README.md)**