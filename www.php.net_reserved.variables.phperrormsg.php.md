# $php_errormsg




<div class="phpcode"><span class="html">
While $php_errormsg is a global, it is not a superglobal.<br><br>You&apos;ll have to qualify it with a global keyword inside a function.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">checkErrormsg</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; global </span><span class="default">$php_errormsg</span><span class="keyword">;<br>&#xA0; &#xA0; @</span><span class="default">strpos</span><span class="keyword">();<br>&#xA0; &#xA0; return </span><span class="default">$php_errormsg</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.phperrormsg.php)

**[⬆ to root](/)**