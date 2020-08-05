# getmypid



The lock-file mechanism in Kevin Trass&apos;s note is incorrect because it is subject to race conditions.<br><br>For locks you need an atomic way of verifying if a lock file exists and creating it if it doesn&apos;t exist. Between file_exists and file_put_contents, another process could be faster than us to write the lock.<br><br>The only filesystem operation that matches the above requirements that I know of is symlink().<br><br>Thus, if you need a lock-file mechanism, here&apos;s the code. This won&apos;t work on a system without /proc (so there go Windows, BSD, OS X, and possibly others), but it can be adapted to work around that deficiency (say, by linking to your pid file like in my script, then operating through the symlink like in Kevin&apos;s solution).<br><br>#!/usr/bin/php<br>

```
<?php

define('LOCK_FILE', "/var/run/" . basename($argv[0], ".php") . ".lock");

if (!tryLock())
    die("Already running.\n");

# remove the lock on exit (Control+C doesn't count as 'exit'?)
register_shutdown_function('unlink', LOCK_FILE);

# The rest of your script goes here....
echo "Hello world!\n";
sleep(30);

exit(0);

function tryLock()
{
    # If lock file exists, check if stale.  If exists and is not stale, return TRUE
    # Else, create lock file and return FALSE.

    if (@symlink("/proc/" . getmypid(), LOCK_FILE) !== FALSE) # the @ in front of 'symlink' is to suppress the NOTICE you get if the LOCK_FILE exists
        return true;

    # link already exists
    # check if it's stale
    if (is_link(LOCK_FILE) &amp;&amp; !is_dir(LOCK_FILE))
    {
        unlink(LOCK_FILE);
        # try to lock again
        return tryLock();
    }

    return false;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.getmypid.php)

**[To root](/README.md)**