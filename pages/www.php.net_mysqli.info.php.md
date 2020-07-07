# mysqli::$info





Might save someone some time...



```
<?php
$prototype=&apos;Rows matched: 0 Changed: 1 Warnings: 2&apos;;
list($matched, $changed, $warnings) = sscanf($prototype, &quot;Rows matched: %d Changed: %d Warnings: %d&quot;);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.info.php)

**[To root](/README.md)**