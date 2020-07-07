# getimagesizefromstring





getimagesizefromstring function for &lt; 5.4



```
<?php
&#xA0;&#xA0; if (!function_exists(&apos;getimagesizefromstring&apos;)) {
&#xA0; &#xA0; &#xA0; function getimagesizefromstring($string_data)
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $uri = &apos;data://application/octet-stream;base64,&apos;&#xA0; . base64_encode($string_data);
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return getimagesize($uri);
&#xA0; &#xA0; &#xA0; }
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.getimagesizefromstring.php)

**[To root](/README.md)**