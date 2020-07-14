# ob_get_level



For users confused about getting "1" as a return value from ob_get_level at the beginning of a script: this likely means the PHP ini directive "output_buffering" is not set to off / 0. PHP automatically starts output buffering for all your scripts if this directive is not off (which acts as if you called ob_start on the first line of your script).<br><br>If your scripts may end up on any server and you don&apos;t want end-users to have to configure their INI, you can use the following at the start of your script to stop output buffering if it&apos;s already started:<br>

```
<?php
if (ob_get_level()) ob_end_clean();
?>
```


Alternatively, you can use the opposite if you always want to have an output buffer at the start of your script:


```
<?php
if (!ob_get_level()) ob_start();
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-get-level.php)

**[To root](/README.md)**