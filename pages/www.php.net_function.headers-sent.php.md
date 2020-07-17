# headers_sent





```
<?php
function redirect($filename) {
    if (!headers_sent())
        header('Location: '.$filename);
    else {
        echo '<script type="text/javascript">';
        echo 'window.location.href="'.$filename.'";';
        echo '</script>';
        echo '<noscript>';
        echo '<meta http-equiv="refresh" content="0;url='.$filename.'" />';
        echo '</noscript>';
    }
}
redirect('http://www.google.com');
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.headers-sent.php)

**[To root](/README.md)**