# getmypid





The lock-file mechanism in Kevin Trass&apos;s note is incorrect because it is subject to race conditions.

For locks you need an atomic way of verifying if a lock file exists and creating it if it doesn&apos;t exist. Between file_exists and file_put_contents, another process could be faster than us to write the lock.

The only filesystem operation that matches the above requirements that I know of is symlink().

Thus, if you need a lock-file mechanism, here&apos;s the code. This won&apos;t work on a system without /proc (so there go Windows, BSD, OS X, and possibly others), but it can be adapted to work around that deficiency (say, by linking to your pid file like in my script, then operating through the symlink like in Kevin&apos;s solution).

#!/usr/bin/php


```
<?php

define(&apos;LOCK_FILE&apos;, &quot;/var/run/&quot; . basename($argv[0], &quot;.php&quot;) . &quot;.lock&quot;);

if (!tryLock())
&#xA0; &#xA0; die(&quot;Already running.\n&quot;);

# remove the lock on exit (Control+C doesn&apos;t count as &apos;exit&apos;?)
register_shutdown_function(&apos;unlink&apos;, LOCK_FILE);

# The rest of your script goes here....
echo &quot;Hello world!\n&quot;;
sleep(30);

exit(0);

function tryLock()
{
&#xA0; &#xA0; # If lock file exists, check if stale.&#xA0; If exists and is not stale, return TRUE
&#xA0; &#xA0; # Else, create lock file and return FALSE.

&#xA0; &#xA0; if (@symlink(&quot;/proc/&quot; . getmypid(), LOCK_FILE) !== FALSE) # the @ in front of &apos;symlink&apos; is to suppress the NOTICE you get if the LOCK_FILE exists
&#xA0; &#xA0; &#xA0; &#xA0; return true;

&#xA0; &#xA0; # link already exists
&#xA0; &#xA0; # check if it&apos;s stale
&#xA0; &#xA0; if (is_link(LOCK_FILE) &amp;&amp; !is_dir(LOCK_FILE))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; unlink(LOCK_FILE);
&#xA0; &#xA0; &#xA0; &#xA0; # try to lock again
&#xA0; &#xA0; &#xA0; &#xA0; return tryLock();
&#xA0; &#xA0; }

&#xA0; &#xA0; return false;
}
php?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.getmypid.php)

**[To root](/README.md)**