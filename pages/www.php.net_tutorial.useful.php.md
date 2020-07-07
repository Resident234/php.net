# Something Useful





Please note that Internet Explorer 11 no longer contains MSIE in its user agent string, for example on Windows 8 with IE11 I get the following:

Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; rv:11.0) like Gecko

So if you want to include a test for IE11, the code above changes to: 



```
<?php
if (strpos($_SERVER[&apos;HTTP_USER_AGENT&apos;], &apos;MSIE&apos;) !== FALSE ||
&#xA0; &#xA0; strpos($_SERVER[&apos;HTTP_USER_AGENT&apos;], &apos;Trident&apos;) !== FALSE) {
&#xA0; &#xA0; echo &apos;You are using Internet Explorer.&lt;br /&gt;&apos;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/tutorial.useful.php)

**[To root](/README.md)**