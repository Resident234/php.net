# opcache_reset





My workaround to clear cache via CLI is create a bash script like this:

#!/bin/bash
WEBDIR=/var/www/html/
RANDOM_NAME=$(head /dev/urandom | tr -dc A-Za-z0-9 | head -c 13)
echo &quot;

```
<?php opcache_reset(); ?>
```
&quot; &gt; ${WEBDIR}${RANDOM_NAME}.php
curl http://localhost/${RANDOM_NAME}.php
rm ${WEBDIR}${RANDOM_NAME}.php

put it in /usr/local/bin/opcache-clear and make it executable. 
When I want to clear cache I simply run &quot;opcache-clear&quot; inside terminal.

  

#



It should be mentioned that opcache_reset() does not reset cache when executed via cli. 
So `php -r &quot;var_dump(opcache_reset());&quot;` outputs &quot;true&quot; but doesn&apos;t clean cache. Make file, access it via http - and cache is clean.

  

#



In some (most?) systems PHP&apos;s CLI has a separate opcode cache to the one used by the web server or PHP-FPM process, which means running opcache_reset() in the CLI won&apos;t reset the webserver/fpm opcode cache, and vice-versa.

  

#

[Official documentation page](https://www.php.net/manual/en/function.opcache-reset.php)

**[To root](/README.md)**