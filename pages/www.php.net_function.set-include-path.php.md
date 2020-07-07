# set_include_path





If you find that this function is failing for you, and you&apos;re not sure why, you may have set your php include path in your sites&apos;s conf file in Apache&#xA0; (this may be true of .htaccess as well)

So to get it to work, comment out any &quot;php_value include_path&quot; type lines in your Apache conf file, and you should be able to set it now in your php code.

  

#



Can be useful to check the value of the constant PATH_SEPARATOR.





```
<?php

if ( ! defined( &quot;PATH_SEPARATOR&quot; ) ) {

&#xA0; if ( strpos( $_ENV[ &quot;OS&quot; ], &quot;Win&quot; ) !== false )

&#xA0; &#xA0; define( &quot;PATH_SEPARATOR&quot;, &quot;;&quot; );

&#xA0; else define( &quot;PATH_SEPARATOR&quot;, &quot;:&quot; );

}

?>
```




For older versions of php, PATH_SEPARATOR is not defined.

If it is so, we must check what kind of OS is on the web-server and define PATH_SEPARATOR properly

  

#

[Official documentation page](https://www.php.net/manual/en/function.set-include-path.php)

**[To root](/README.md)**