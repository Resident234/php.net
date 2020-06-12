# touch




<div class="phpcode"><span class="html">
Note that when PHP is called by f.e. apache or nginx instead of directly from the command line, touch() will not prefix the location of the invoking script, so the supplied filename must contain an absolute path.<br><br>With script started from /home/user/www, this will not touch &quot;/home/user/www/somefile&quot;:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; touch</span><span class="keyword">( </span><span class="string">&apos;somefile&apos; </span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>But this will:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; touch</span><span class="keyword">( </span><span class="default">__DIR__ </span><span class="keyword">. </span><span class="string">&apos;/somefile&apos; </span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve been trying to set a filemtime into the future with touch() on PHP5.<br><br>It seems touch $time has a future limit around 1000000 seconds (11 days or so). Beyond this point it reverts to a previous $time.<br><br>It doesn&apos;t make much sense but I could save you hours of time.<br><br>$time = time()+1500000;<br>touch($cachedfile,$time);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.touch.php)

**[⬆ to root](/)**