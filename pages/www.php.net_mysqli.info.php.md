# mysqli::$info



Might save someone some time...<br><br>

```
<?php
$prototype='Rows matched: 0 Changed: 1 Warnings: 2';
list($matched, $changed, $warnings) = sscanf($prototype, "Rows matched: %d Changed: %d Warnings: %d");
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.info.php)

**[To root](/README.md)**