# zip_open



Note that the Zip functions return an integer error number in the event of error. So:<br><br>

```
<?php
$zip = zip_open($file);

if ($zip) {
?>
```


is incorrect. Instead use:



```
<?php
$zip = zip_open($file);

if (is_resource($zip)) {
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.zip-open.php)

**[To root](/README.md)**