# headers_sent





```
<?php
function redirect($filename) {
    if (!headers_sent())
        header('Location: '.$filename);
    else {
        echo '&lt;script type="text/javascript"&gt;';
        echo 'window.location.href="'.$filename.'";';
        echo '&lt;/script&gt;';
        echo '&lt;noscript&gt;';
        echo '&lt;meta http-equiv="refresh" content="0;url='.$filename.'" /&gt;';
        echo '&lt;/noscript&gt;';
    }
}
redirect('http://www.google.com');
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.headers-sent.php)

**[To root](/README.md)**