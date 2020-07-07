# ob_get_contents





This is an example of how the stack works:



```
<?php
//Level 0
ob_start();
echo &quot;Hello &quot;;

//Level 1
ob_start();
echo &quot;Hello World&quot;;
$out2 = ob_get_contents();
ob_end_clean();

//Back to level 0
echo &quot;Galaxy&quot;;
$out1 = ob_get_contents();
ob_end_clean();

//Just output
var_dump($out1, $out2);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-get-contents.php)

**[To root](/README.md)**