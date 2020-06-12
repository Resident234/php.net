# filemtime




<div class="phpcode"><span class="html">
This is a very handy function for dealing with browser caching. For example, say you have a stylesheet and you want to make sure everyone has the most recent version. You could rename it every time you edit it, but that would be a pain in the ass. Instead, you can do this:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">echo </span><span class="string">&apos;&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot; href=&quot;style.css?&apos; </span><span class="keyword">. </span><span class="default">filemtime</span><span class="keyword">(</span><span class="string">&apos;style.css&apos;</span><span class="keyword">) . </span><span class="string">&apos;&quot; /&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Sample output:
<br>
<br>&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot; href=&quot;style.css?1203291283&quot; /&gt;
<br>
<br>By appending a GET value (the UNIX timestamp) to the stylesheet URL, you make the browser think the stylesheet is dynamic, so it&apos;ll reload the stylesheet every time the modification date changes.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To get the last modification time of a directory, you can use this:<br><br>&lt;pre&gt;<br>$getLastModDir = filemtime(&quot;/path/to/directory/.&quot;);<br>&lt;/pre&gt;<br><br>Take note on the last dot which is needed to see the directory as a file and to actually get a last modification date of it.<br><br>This comes in handy when you want just one &apos;last updated&apos; message on the frontpage of your website and still taking all files of your website into account.<br><br>Regards,<br>Frank Keijzers</span>
</div>
  

#


<div class="phpcode"><span class="html">
Cheaper and dirtier way to code a cache:<br><br><span class="default">&lt;?php<br>$cache_file </span><span class="keyword">= </span><span class="string">&apos;URI to cache file&apos;</span><span class="keyword">;<br></span><span class="default">$cache_life </span><span class="keyword">= </span><span class="string">&apos;120&apos;</span><span class="keyword">; </span><span class="comment">//caching time, in seconds<br><br></span><span class="default">$filemtime </span><span class="keyword">= @</span><span class="default">filemtime</span><span class="keyword">(</span><span class="default">$cache_file</span><span class="keyword">);&#xA0; </span><span class="comment">// returns FALSE if file does not exist<br></span><span class="keyword">if (!</span><span class="default">$filemtime </span><span class="keyword">or (</span><span class="default">time</span><span class="keyword">() - </span><span class="default">$filemtime </span><span class="keyword">&gt;= </span><span class="default">$cache_life</span><span class="keyword">)){<br>&#xA0; &#xA0; </span><span class="default">ob_start</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">resource_consuming_function</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">file_put_contents</span><span class="keyword">(</span><span class="default">$cache_file</span><span class="keyword">,</span><span class="default">ob_get_flush</span><span class="keyword">());<br>}else{<br>&#xA0; &#xA0; </span><span class="default">readfile</span><span class="keyword">(</span><span class="default">$cache_file</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There&apos;s a deeply-seated problem with filemtime() under Windows due to the fact that it calls Windows&apos; stat() function, which implements DST (according to this bug: <a href="http://bugs.php.net/bug.php?id=40568" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=40568</a>). The detection of DST on the time of the file is confused by whether the CURRENT time of the current system is currently under DST.
<br>
<br>This is a fix for the mother of all annoying bugs:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">GetCorrectMTime</span><span class="keyword">(</span><span class="default">$filePath</span><span class="keyword">)
<br>{
<br>
<br>&#xA0; &#xA0; </span><span class="default">$time </span><span class="keyword">= </span><span class="default">filemtime</span><span class="keyword">(</span><span class="default">$filePath</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$isDST </span><span class="keyword">= (</span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;I&apos;</span><span class="keyword">, </span><span class="default">$time</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$systemDST </span><span class="keyword">= (</span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;I&apos;</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$adjustment </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if(</span><span class="default">$isDST </span><span class="keyword">== </span><span class="default">false </span><span class="keyword">&amp;&amp; </span><span class="default">$systemDST </span><span class="keyword">== </span><span class="default">true</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$adjustment </span><span class="keyword">= </span><span class="default">3600</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; else if(</span><span class="default">$isDST </span><span class="keyword">== </span><span class="default">true </span><span class="keyword">&amp;&amp; </span><span class="default">$systemDST </span><span class="keyword">== </span><span class="default">false</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$adjustment </span><span class="keyword">= -</span><span class="default">3600</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$adjustment </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; return (</span><span class="default">$time </span><span class="keyword">+ </span><span class="default">$adjustment</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Dustin Oprea</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.filemtime.php)

**[⬆ to root](/)**