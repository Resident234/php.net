# Output Buffering Control



The manual is a little shy on explaining that output buffers are nested, and that "turns off output buffering" means turning off the highest nested buffer.  See ob_get_level (for a useful function, but still no explanation)<br><br>

```
<?php
    ob_start();
    echo "1:blah\n";
    ob_start();
    echo "2:blah";
    // ob_get_clean() returns the contents of the last buffer opened.  The first "blah" and the output of var_dump are flushed from the top buffer on exit
    var_dump(ob_get_clean());
    exit;
?>
```
<br><br>puts:<br>    1:blah<br>    string(6) "2:blah"<br><br>Prior to realising this, I had thought PHP&apos;s ob functionality left more to be desired.  I *really* wish I knew earlier.  

#

[Official documentation page](https://www.php.net/manual/en/book.outcontrol.php)

**[To root](/README.md)**