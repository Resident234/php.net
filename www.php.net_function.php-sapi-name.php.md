# php_sapi_name




<div class="phpcode"><span class="html">
The php_sapi_name() function is extremely useful when you want to determine the type of interface. There is, however, one more gotcha you need to be aware of while designing your application or deploying it to an unknown server.<br><br>Whenever something depends on the type of interface, make sure your check is conclusive. Especially when you want to distinguish the command line interface (CLI) from the common gateway interface (CGI).<br><br>Note, that the php-cgi binary can be called from the command line, from a shell script or as a cron job as well! If so, the php_sapi_name() will always return the same value (i.e. &quot;cgi-fcgi&quot;) instead of &quot;cli&quot; which you could expect.<br><br>Bad things happen to good people. Do not always expect /usr/bin/php to be a link to php-cli binary.<br><br>Luckily the contents of the $_SERVER and the $_ENV superglobal arrays depends on whether the php-cgi binary is called from the command line interface (by a shell script, by the cron, etc.) or by some HTTP server (i.e. lighttpd).<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Try to call php-cgi binary from the command line interface and then via HTTP request and compare the output of the script above. There will be plenty options to satisfy almost everyone.<br><br>For the sake of security remember, that contents of the $_SERVER and the $_ENV superglobal arrays (as well as $_GET, $_POST, $_COOKIE, $_FILES and $_REQUEST) should be considered tainted.</span>
</div>
  

#


<div class="phpcode"><span class="html">
some not yet mentioned sapi names:<br><br>cli-server -&gt; php built-in webserver<br>srv -&gt; hhvm</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.php-sapi-name.php)

**[To root](/)**