# Memcached::getVersion




<div class="phpcode"><span class="html">
Was going mad, I figured getStatus() would return false on servers not responding. This is however not true, it will return:<br>array(1) {<br>&#xA0; [&quot;127.0.0.1:11112&quot;]=&gt;<br>&#xA0; string(11) &quot;255.255.255&quot;<br>}<br>On a failed connection.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.getversion.php)

**[To root](/README.md)**