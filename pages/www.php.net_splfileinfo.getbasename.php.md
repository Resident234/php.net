# SplFileInfo::getBasename



If you want to get only filename and dont want to use weird:<br><br>

```
<?php
pathinfo($file-&gt;getBasename(), PATHINFO_FILENAME);
?>
```


You can use (also weird but ~better looking):



```
<?php
$file-&gt;getBasename(&apos;.&apos;.$file-&gt;getExtension());
?>
```
<br><br>PS: Why there is getFilename ? when it returns ~same stuff as getBasename ? I have to do this ugly stuff^ instead of simple getFilename...  

#

[Official documentation page](https://www.php.net/manual/en/splfileinfo.getbasename.php)

**[To root](/README.md)**