# Runtime Configuration





I&apos;m surprised this isn&apos;t mentioned in docs here, but to set these values at runtime use &quot;ini_set()&quot;. For example:



```
<?php
ini_set(&quot;auto_detect_line_endings&quot;, true);

// Now I can invoke fgets() on files that contain silly \r line endings. 
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/filesystem.configuration.php)

**[To root](/README.md)**