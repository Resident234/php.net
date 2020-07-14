# getenv



Contrary to what eng.mrkto.com said, getenv() isn&apos;t always case-insensitive. On Linux it is not:<br><br>

```
<?php
var_dump(getenv('path')); // bool(false)
var_dump(getenv('Path')); // bool(false)
var_dump(getenv('PATH')); // string(13) "/usr/bin:/bin"?>
```
  

#

This function is useful (compared to $_SERVER, $_ENV) because it searches $varname key in those array case-insensitive manner.<br>For example on Windows $_SERVER[&apos;Path&apos;] is like you see Capitalized, not &apos;PATH&apos; as you expected.<br>So just: 

```
<?php getenv('path') ?>
```
  

#

All of the notes and examples so far have been strictly CGI.<br>It should not be understated the usefulness of getenv()/putenv() in CLI as well.<br><br>You can pass a number of variables to a CLI script via environment variables, either in Unix/Linux bash/sh with the "VAR=&apos;foo&apos;; export $VAR" paradigm, or in Windows with the "set VAR=&apos;foo&apos;" paradigm. (Csh users, you&apos;re on your own!) getenv("VAR") will retrieve that value from the environment.<br><br>We have a system by which we include a file full of putenv() statements storing configuration values that can apply to many different CLI PHP programs. But if we want to override these values, we can use the shell&apos;s (or calling application, such as ant) environment variable setting method to do so.<br><br>This saves us from having to manage an unmanageable amount of one-off configuration changes per execution via command line arguments; instead we just set the appropriate env var first.  

#

[Official documentation page](https://www.php.net/manual/en/function.getenv.php)

**[To root](/README.md)**