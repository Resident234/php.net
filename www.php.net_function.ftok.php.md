# ftok




<div class="phpcode"><span class="html">
Thanks to&#xA0; daniele_dll@yahoo.it who got this in turn from linux glibc 2.3.2: <a href="http://www.php.net/manual/en/function.shmop-open.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.shmop-open.php</a> -- I&apos;m putting this here because it might be helpful to others.<br><br>function ftok($pathname, $proj_id) {<br>&#xA0;&#xA0; $st = @stat($pathname);<br>&#xA0;&#xA0; if (!$st) {<br>&#xA0; &#xA0; &#xA0;&#xA0; return -1;<br>&#xA0;&#xA0; }<br>&#xA0; <br>&#xA0;&#xA0; $key = sprintf(&quot;%u&quot;, (($st[&apos;ino&apos;] &amp; 0xffff) | (($st[&apos;dev&apos;] &amp; 0xff) &lt;&lt; 16) | (($proj_id &amp; 0xff) &lt;&lt; 24)));<br>&#xA0;&#xA0; return $key;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftok.php)

**[⬆ to root](/)**