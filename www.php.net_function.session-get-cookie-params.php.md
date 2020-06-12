# session_get_cookie_params




<div class="phpcode"><span class="html">
It should be noted that this gets the session cookie ini file parameters, not the parameters from the cookie itself.<br><br>ie. if you set the cookie lifetime using session_set_cookie_params(12345) and then try to use session_get_cookie_params, you will not get back 12345. Instead, you will get the lifetime set in the ini file.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-get-cookie-params.php)

**[⬆ to root](/)**