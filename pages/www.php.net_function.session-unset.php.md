# session_unset



I was having a problem clearing all session variables, deleting the session, and creating a new session without leaving old session stuff behind in all browsers.  The below code is perfect for a logout script to totally delete everything and start new.  It even works in Chrome which seems to not work as other browsers when trying do logout and start a new session.<br><br>

```
<?php
    session_start();
    session_unset();
    session_destroy();
    session_write_close();
    setcookie(session_name(),&apos;&apos;,0,&apos;/&apos;);
    session_regenerate_id(true);
?>
```
  

#

The difference between both session_unset and session_destroy is as follows:<br><br>session_unset just clears out the session for usage. The session is still on the users computer. Note that by using session_unset, the variable still exists. session_unset just remove all session variables. it does not destroy the session....so the session would still be active.<br><br>Using session_unset in tandem with session_destroy however, is a much more effective means of actually clearing out data. As stated in the example above, this works very well, cross browser. session_destroy is destroy the session. session_destroy() to kill all session information.....This is the more secure function to use.  

#

[Official documentation page](https://www.php.net/manual/en/function.session-unset.php)

**[To root](/README.md)**