# set_include_path



If you find that this function is failing for you, and you&apos;re not sure why, you may have set your php include path in your sites&apos;s conf file in Apache  (this may be true of .htaccess as well)<br><br>So to get it to work, comment out any "php_value include_path" type lines in your Apache conf file, and you should be able to set it now in your php code.  

---

Can be useful to check the value of the constant PATH_SEPARATOR.<br><br>

```
<?php
if ( ! defined( "PATH_SEPARATOR" ) ) {
  if ( strpos( $_ENV[ "OS" ], "Win" ) !== false )
    define( "PATH_SEPARATOR", ";" );
  else define( "PATH_SEPARATOR", ":" );
}
?>
```
<br><br>For older versions of php, PATH_SEPARATOR is not defined.<br>If it is so, we must check what kind of OS is on the web-server and define PATH_SEPARATOR properly  

---

[Official documentation page](https://www.php.net/manual/en/function.set-include-path.php)

**[To root](/README.md)**