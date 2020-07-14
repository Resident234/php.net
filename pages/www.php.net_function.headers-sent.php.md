# headers_sent





```
<?php
function redirect($filename) {
    if (!headers_sent())
        header(&apos;Location: &apos;.$filename);
    else {
        echo &apos;&lt;script type="text/javascript"&gt;&apos;;
        echo &apos;window.location.href="&apos;.$filename.&apos;";&apos;;
        echo &apos;&lt;/script&gt;&apos;;
        echo &apos;&lt;noscript&gt;&apos;;
        echo &apos;&lt;meta http-equiv="refresh" content="0;url=&apos;.$filename.&apos;" /&gt;&apos;;
        echo &apos;&lt;/noscript&gt;&apos;;
    }
}
redirect(&apos;http://www.google.com&apos;);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.headers-sent.php)

**[To root](/README.md)**