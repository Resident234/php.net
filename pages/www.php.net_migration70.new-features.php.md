# New features





```
<?php 
class foo { static $bar = &apos;baz&apos;; }
var_dump(&apos;foo&apos;::$bar);
?>
```
<br><br>if &lt; php7.0<br><br>then we will receive a syntax error, unexpected &apos;::&apos; (T_PAAMAYIM_NEKUDOTAYIM) <br><br>but php7 returns string(3) "baz"<br><br>I think it&apos;s not documented anywhere  

#

[Official documentation page](https://www.php.net/manual/en/migration70.new-features.php)

**[To root](/README.md)**