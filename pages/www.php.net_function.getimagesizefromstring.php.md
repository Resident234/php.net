# getimagesizefromstring



getimagesizefromstring function for &lt; 5.4<br><br>

```
<?php
   if (!function_exists(&apos;getimagesizefromstring&apos;)) {
      function getimagesizefromstring($string_data)
      {
         $uri = &apos;data://application/octet-stream;base64,&apos;  . base64_encode($string_data);
         return getimagesize($uri);
      }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.getimagesizefromstring.php)

**[To root](/README.md)**