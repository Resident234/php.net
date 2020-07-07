# ArgumentCountError





Note if an invalid number of arguments are passed to a built-in function an ArgumentCountError exception will be thrown if and only if your code is in strict mode.



```
<?php
declare(strict_types = 1);

try {
&#xA0; &#xA0; echo strlen(&apos;ahmed&apos;, 4);
} catch (ArgumentCountError $e) {
&#xA0; &#xA0; echo $e-&gt;getMessage()&apos;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.argumentcounterror.php)

**[To root](/README.md)**