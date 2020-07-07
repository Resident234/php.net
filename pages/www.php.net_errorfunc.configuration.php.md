# Runtime Configuration





Using 



```
<?php ini_set(&apos;display_errors&apos;, 1); ?>
```
 

at the top of your script will not catch any parse errors. A missing &quot;)&quot; or &quot;;&quot; will still lead to a blank page.



This is because the entire script is parsed before any of it is executed. If you are unable to change php.ini and set



display_errors On



then there is a possible solution suggested under error_reporting:





```
<?php

 error_reporting(E_ALL);

 ini_set(&quot;display_errors&quot;, 1);

 include(&quot;file_with_errors.php&quot;);

?>
```






[Modified by moderator]



You should also consider setting error_reporting = -1 in your php.ini and display_errors = On if you are in development mode to see all fatal/parse errors or set error_log to your desired file to log errors instead of display_errors in production (this requires log_errors to be turned on).

  

#



If you set the error_log directive to a relative path, it is a path relative to the document root rather than php&apos;s containing folder.

  

#

[Official documentation page](https://www.php.net/manual/en/errorfunc.configuration.php)

**[To root](/README.md)**