# Session Handling




<div class="phpcode"><span class="html">
There is a nuance we found with session timing out although the user is still active in the session.&#xA0; The problem has to do with never modifying the session variable.
<br>
<br>The GC will clear the session data files based on their last modification time.&#xA0; Thus if you never modify the session, you simply read from it, then the GC will eventually clean up.
<br>
<br>To prevent this you need to ensure that your session is modified within the GC delete time.&#xA0; You can accomplish this like below.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if( !isset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;last_access&apos;</span><span class="keyword">]) || (</span><span class="default">time</span><span class="keyword">() - </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;last_access&apos;</span><span class="keyword">]) &gt; </span><span class="default">60 </span><span class="keyword">)
<br>&#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;last_access&apos;</span><span class="keyword">] = </span><span class="default">time</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>This will update the session every 60s to ensure that the modification date is altered.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.session.php)

**[⬆ to root](/)**