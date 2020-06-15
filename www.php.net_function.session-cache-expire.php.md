# session_cache_expire




<div class="phpcode"><span class="html">
The manual probably doesn&apos;t stress this enough:<br><br>** This has nothing to do with lifetime of a session **<br><br>Whatever you set this setting to, it won&apos;t change how long sessions live on your server.<br><br>This only changes HTTP cache expiration time (Expires: and Cache-Control: max-age headers), which advise browser for how long it can keep pages cached in user&apos;s cache without having to reload them from the server.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-cache-expire.php)

**[To root](/README.md)**