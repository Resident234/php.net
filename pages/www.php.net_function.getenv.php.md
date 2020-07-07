# getenv





Contrary to what eng.mrkto.com said, getenv() isn&apos;t always case-insensitive. On Linux it is not:



```
<?php
var_dump(getenv(&apos;path&apos;)); // bool(false)
var_dump(getenv(&apos;Path&apos;)); // bool(false)
var_dump(getenv(&apos;PATH&apos;)); // string(13) &quot;/usr/bin:/bin&quot;


  

#



This function is useful (compared to $_SERVER, $_ENV) because it searches $varname key in those array case-insensitive manner.
For example on Windows $_SERVER[&apos;Path&apos;] is like you see Capitalized, not &apos;PATH&apos; as you expected.
So just: 

```
<?php getenv(&apos;path&apos;) php?>
```



  

#



All of the notes and examples so far have been strictly CGI.
It should not be understated the usefulness of getenv()/putenv() in CLI as well.

You can pass a number of variables to a CLI script via environment variables, either in Unix/Linux bash/sh with the &quot;VAR=&apos;foo&apos;; export $VAR&quot; paradigm, or in Windows with the &quot;set VAR=&apos;foo&apos;&quot; paradigm. (Csh users, you&apos;re on your own!) getenv(&quot;VAR&quot;) will retrieve that value from the environment.

We have a system by which we include a file full of putenv() statements storing configuration values that can apply to many different CLI PHP programs. But if we want to override these values, we can use the shell&apos;s (or calling application, such as ant) environment variable setting method to do so.

This saves us from having to manage an unmanageable amount of one-off configuration changes per execution via command line arguments; instead we just set the appropriate env var first.

  

#

[Official documentation page](https://www.php.net/manual/en/function.getenv.php)

**[To root](/README.md)**