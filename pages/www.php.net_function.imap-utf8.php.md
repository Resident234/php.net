# imap_utf8





That fixed the all caps issue:





```
<?php

function imap_utf8_fix($string) {

&#xA0; return iconv_mime_decode($string,0,&quot;UTF-8&quot;);

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-utf8.php)

**[To root](/README.md)**