# $php_errormsg



While $php_errormsg is a global, it is not a superglobal.<br><br>You&apos;ll have to qualify it with a global keyword inside a function.<br><br>

```
<?php
function checkErrormsg()
{
    global $php_errormsg;
    @strpos();
    return $php_errormsg;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.phperrormsg.php)

**[To root](/README.md)**