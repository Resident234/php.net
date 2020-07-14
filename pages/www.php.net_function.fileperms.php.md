# fileperms



Don&apos;t use substr, use bit operator<br>

```
<?php
decoct(fileperms($file) &amp; 0777); // return "755" for example
?>
```


If you want to compare permission


```
<?php
0755 === (fileperms($file) &amp; 0777);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.fileperms.php)

**[To root](/README.md)**