# fastcgi_finish_request




<div class="phpcode"><span class="html">
There are some pitfalls&#xA0; you should be aware of when using this function.<br><br>The script will still occupy a FPM process after fastcgi_finish_request(). So using it excessively for long running tasks may occupy all your FPM threads up to pm.max_children. This will lead to gateway errors on the webserver.<br><br>Another important thing is session handling. Sessions are locked as long as they&apos;re active (see the documentation for session_write_close()). This means subsequent requests will block until the session is closed.<br><br>You should therefore call session_write_close() as soon as possible (even before fastcgi_finish_request()) to allow subsequent requests and a good user experience.<br><br>This also applies for all other locking techniques as flock or database locks for example. As long as a lock is active subsequent requests might bock.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fastcgi-finish-request.php)

**[⬆ to root](/)**