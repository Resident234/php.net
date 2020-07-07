# ftruncate





If you want to empty a file of it&apos;s contents bare in mind that opening a file in w mode truncates the file automatically, so instead of doing...



```
<?php
$fp = fopen(&quot;/tmp/file.txt&quot;, &quot;r+&quot;);
ftruncate($fp, 0);
fclose($fp);
?>
```


You can just do...



```
<?php
$fp = fopen(&quot;/tmp/file.txt&quot;, &quot;w&quot;);
fclose($fp);
?>
```



  

#



You MUST use rewind() after ftruncate() to replace file content

  

#

[Official documentation page](https://www.php.net/manual/en/function.ftruncate.php)

**[To root](/README.md)**