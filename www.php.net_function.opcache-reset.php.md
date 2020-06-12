# opcache_reset




<div class="phpcode"><span class="html">
My workaround to clear cache via CLI is create a bash script like this:<br><br>#!/bin/bash<br>WEBDIR=/var/www/html/<br>RANDOM_NAME=$(head /dev/urandom | tr -dc A-Za-z0-9 | head -c 13)<br>echo &quot;<span class="default">&lt;?php opcache_reset</span><span class="keyword">(); </span><span class="default">?&gt;</span>&quot; &gt; ${WEBDIR}${RANDOM_NAME}.php<br>curl <a href="http://localhost/${RANDOM_NAME}.php" rel="nofollow" target="_blank">http://localhost/${RANDOM_NAME}.php</a><br>rm ${WEBDIR}${RANDOM_NAME}.php<br><br>put it in /usr/local/bin/opcache-clear and make it executable. <br>When I want to clear cache I simply run &quot;opcache-clear&quot; inside terminal.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be mentioned that opcache_reset() does not reset cache when executed via cli. <br>So `php -r &quot;var_dump(opcache_reset());&quot;` outputs &quot;true&quot; but doesn&apos;t clean cache. Make file, access it via http - and cache is clean.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In some (most?) systems PHP&apos;s CLI has a separate opcode cache to the one used by the web server or PHP-FPM process, which means running opcache_reset() in the CLI won&apos;t reset the webserver/fpm opcode cache, and vice-versa.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.opcache-reset.php)

**[To root](/)**