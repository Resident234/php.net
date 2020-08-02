# base64_decode



If you want to save data that is derived from a Javascript canvas.toDataURL() function, you have to convert blanks into plusses. If you do not do that, the decoded data is corrupted:<br><br>

```
<?php
  $encodedData = str_replace(' ','+',$encodedData);
  $decocedData = base64_decode($encodedData);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.base64-decode.php)

**[To root](/README.md)**