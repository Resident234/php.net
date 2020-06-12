# session_name




<div class="phpcode"><span class="html">
This may sound no-brainer: the session_name() function will have no essential effect if you set session.auto_start to &quot;true&quot; in php.ini . And the obvious explanation is the session already started thus cannot be altered before the session_name() function--wherever it is in the script--is executed, same reason session_name needs to be called before session_start() as documented.<br><br>I know it is really not a big deal. But I had a quite hard time before figuring this out, and hope it might be helpful to someone like me.</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you try to name a php session &quot;example.com&quot; it gets converted to &quot;example_com&quot; and everything breaks.<br><br>don&apos;t use a period in your session name.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Remember, kids--you MUST use session_name() first if you want to use session_set_cookie_params() to, say, change the session timeout. Otherwise it won&apos;t work, won&apos;t give any error, and nothing in the documentation (that I&apos;ve seen, anyway) will explain why.<br><br>Thanks to brandan of bildungsroman.com who left a note under session_set_cookie_params() explaining this or I&apos;d probably still be throwing my hands up about it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those wondering, this function is expensive!<br><br>On a script that was executing in a consistent 0.0025 seconds, just the use of session_name(&quot;foo&quot;) shot my execution time up to ~0.09s. By simply sacrificing session_name(&quot;foo&quot;), I sped my script up by roughly 0.09 seconds.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-name.php)

**[⬆ to root](/)**