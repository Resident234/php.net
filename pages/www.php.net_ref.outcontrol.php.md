# Output Control Functions





It seems that while using output buffering, an included file which calls die() before the output buffer is closed is flushed rather than cleaned. That is, ob_end_flush() is called by default.



```
<?php
// a.php (this file should never display anything)
ob_start();
include(&apos;b.php&apos;);
ob_end_clean();
?>
```




```
<?php
// b.php
print &quot;b&quot;;
die();
?>
```


This ends up printing &quot;b&quot; rather than nothing as ob_end_flush() is called instead of ob_end_clean(). That is, die() flushes the buffer rather than cleans it. This took me a while to determine what was causing the flush, so I thought I&apos;d share.

  

#

[Official documentation page](https://www.php.net/manual/en/ref.outcontrol.php)

**[To root](/README.md)**