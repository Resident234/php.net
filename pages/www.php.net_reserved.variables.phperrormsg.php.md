# $php_errormsg





While $php_errormsg is a global, it is not a superglobal.

You&apos;ll have to qualify it with a global keyword inside a function.



```
<?php
function checkErrormsg()
{
&#xA0; &#xA0; global $php_errormsg;
&#xA0; &#xA0; @strpos();
&#xA0; &#xA0; return $php_errormsg;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.phperrormsg.php)

**[To root](/README.md)**