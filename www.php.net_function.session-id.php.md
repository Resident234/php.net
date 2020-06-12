# session_id




<div class="phpcode"><span class="html">
It may be good to note that PHP does not allow arbitrary session ids. The session id validation in PHP source is defined in ext/session/session.c in the function php_session_valid_key:<br><br><a href="https://github.com/php/php-src/blob/master/ext/session/session.c" rel="nofollow" target="_blank">https://github.com/php/php-src/blob/master/ext/session/session.c</a><br><br>To put it short, a valid session id may consists of digits, letters A to Z (both upper and lower case), comma and dash. Described as a character class, it would be [-,a-zA-Z0-9]. A valid session id may have the length between 1 and 128 characters. To validate session ids, the easiest way to do it use a function like:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">session_valid_id</span><span class="keyword">(</span><span class="default">$session_id</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/^[-,a-zA-Z0-9]{1,128}$/&apos;</span><span class="keyword">, </span><span class="default">$session_id</span><span class="keyword">) &gt; </span><span class="default">0</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>session_id() itself will happily accept invalid session ids, but if you try to start a session using an invalid id, you will get the following error:<br><br>Warning: session_start(): The session id is too long or contains illegal characters, valid characters are a-z, A-Z, 0-9 and &apos;-,&apos;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-id.php)

**[⬆ to root](/)**