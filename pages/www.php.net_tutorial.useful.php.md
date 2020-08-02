# Something Useful



Please note that Internet Explorer 11 no longer contains MSIE in its user agent string, for example on Windows 8 with IE11 I get the following:<br><br>Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; rv:11.0) like Gecko<br><br>So if you want to include a test for IE11, the code above changes to: <br><br>

```
<?php
if (strpos($_SERVER['HTTP_USER_AGENT'], 'MSIE') !== FALSE ||
    strpos($_SERVER['HTTP_USER_AGENT'], 'Trident') !== FALSE) {
    echo 'You are using Internet Explorer.<br />';
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/tutorial.useful.php)

**[To root](/README.md)**