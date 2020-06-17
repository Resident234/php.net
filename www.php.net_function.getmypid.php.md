# getmypid




<div class="phpcode"><span class="html">
The lock-file mechanism in Kevin Trass&apos;s note is incorrect because it is subject to race conditions.<br><br>For locks you need an atomic way of verifying if a lock file exists and creating it if it doesn&apos;t exist. Between file_exists and file_put_contents, another process could be faster than us to write the lock.<br><br>The only filesystem operation that matches the above requirements that I know of is symlink().<br><br>Thus, if you need a lock-file mechanism, here&apos;s the code. This won&apos;t work on a system without /proc (so there go Windows, BSD, OS X, and possibly others), but it can be adapted to work around that deficiency (say, by linking to your pid file like in my script, then operating through the symlink like in Kevin&apos;s solution).<br><br>#!/usr/bin/php<br><span class="default">&lt;?php<br><br>define</span><span class="keyword">(</span><span class="string">&apos;LOCK_FILE&apos;</span><span class="keyword">, </span><span class="string">&quot;/var/run/&quot; </span><span class="keyword">. </span><span class="default">basename</span><span class="keyword">(</span><span class="default">$argv</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="string">&quot;.php&quot;</span><span class="keyword">) . </span><span class="string">&quot;.lock&quot;</span><span class="keyword">);<br><br>if (!</span><span class="default">tryLock</span><span class="keyword">())<br>&#xA0; &#xA0; die(</span><span class="string">&quot;Already running.\n&quot;</span><span class="keyword">);<br><br></span><span class="comment"># remove the lock on exit (Control+C doesn&apos;t count as &apos;exit&apos;?)<br></span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;unlink&apos;</span><span class="keyword">, </span><span class="default">LOCK_FILE</span><span class="keyword">);<br><br></span><span class="comment"># The rest of your script goes here....<br></span><span class="keyword">echo </span><span class="string">&quot;Hello world!\n&quot;</span><span class="keyword">;<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">30</span><span class="keyword">);<br><br>exit(</span><span class="default">0</span><span class="keyword">);<br><br>function </span><span class="default">tryLock</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; </span><span class="comment"># If lock file exists, check if stale.&#xA0; If exists and is not stale, return TRUE<br>&#xA0; &#xA0; # Else, create lock file and return FALSE.<br><br>&#xA0; &#xA0; </span><span class="keyword">if (@</span><span class="default">symlink</span><span class="keyword">(</span><span class="string">&quot;/proc/&quot; </span><span class="keyword">. </span><span class="default">getmypid</span><span class="keyword">(), </span><span class="default">LOCK_FILE</span><span class="keyword">) !== </span><span class="default">FALSE</span><span class="keyword">) </span><span class="comment"># the @ in front of &apos;symlink&apos; is to suppress the NOTICE you get if the LOCK_FILE exists<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">true</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment"># link already exists<br>&#xA0; &#xA0; # check if it&apos;s stale<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">is_link</span><span class="keyword">(</span><span class="default">LOCK_FILE</span><span class="keyword">) &amp;&amp; !</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">LOCK_FILE</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">LOCK_FILE</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment"># try to lock again<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">tryLock</span><span class="keyword">();<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.getmypid.php)

**[To root](/README.md)**