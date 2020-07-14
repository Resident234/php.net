# session_name



This may sound no-brainer: the session_name() function will have no essential effect if you set session.auto_start to "true" in php.ini . And the obvious explanation is the session already started thus cannot be altered before the session_name() function--wherever it is in the script--is executed, same reason session_name needs to be called before session_start() as documented.<br><br>I know it is really not a big deal. But I had a quite hard time before figuring this out, and hope it might be helpful to someone like me.  

#

if you try to name a php session "example.com" it gets converted to "example_com" and everything breaks.<br><br>don&apos;t use a period in your session name.  

#

Remember, kids--you MUST use session_name() first if you want to use session_set_cookie_params() to, say, change the session timeout. Otherwise it won&apos;t work, won&apos;t give any error, and nothing in the documentation (that I&apos;ve seen, anyway) will explain why.<br><br>Thanks to brandan of bildungsroman.com who left a note under session_set_cookie_params() explaining this or I&apos;d probably still be throwing my hands up about it.  

#

For those wondering, this function is expensive!<br><br>On a script that was executing in a consistent 0.0025 seconds, just the use of session_name("foo") shot my execution time up to ~0.09s. By simply sacrificing session_name("foo"), I sped my script up by roughly 0.09 seconds.  

#

[Official documentation page](https://www.php.net/manual/en/function.session-name.php)

**[To root](/README.md)**