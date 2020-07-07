# getcwd





getcwd() returns the path of the &quot;main&quot; script referenced in the URL.

dirname(__FILE__) will return the path of the script currently executing.

I had written a script that required several class definition scripts from the same directory. It retrieved them based on filename matches and used getcwd to figure out where they were.

Didn&apos;t work so well when I needed to call that first script from a new file in a different directory.

  

#



It appears there is a change in functionality in PHP5 from PHP4 when using the CLI tool.&#xA0; Here is the example: -

cd /tmp

cat &gt; foo.php &lt;&lt; END


```
<?php
&#xA0; &#xA0; print getcwd() . &quot;\n&quot;;
?>
```

END

cd /

php -q /tmp/foo.php

PHP4 returns /tmp
PHP5 returns /

Something to be aware of.

  

#

[Official documentation page](https://www.php.net/manual/en/function.getcwd.php)

**[To root](/README.md)**