# ob_get_clean





The definition should mention that the function also &quot;turns off output buffering&quot;, not just cleans it.

  

#



Also, don&apos;t forget that you will need to ob_start() again for any successive calls:





```
<?php

ob_start();

echo &quot;1&quot;;

$content = ob_get_clean();



ob_start(); // This is NECESSARY for the next ob_get_clean() to work as intended.

echo &quot;2&quot;;

$content .= ob_get_clean();



echo $content;

php?>
```




Output: 12



Without the second ob_start(), the output is 21 ...

  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-get-clean.php)

**[To root](/README.md)**