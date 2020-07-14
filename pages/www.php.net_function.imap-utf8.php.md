# imap_utf8



That fixed the all caps issue:<br><br>

```
<?php
function imap_utf8_fix($string) {
  return iconv_mime_decode($string,0,"UTF-8");
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-utf8.php)

**[To root](/README.md)**