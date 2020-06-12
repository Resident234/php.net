# $_REQUEST




<div class="phpcode"><span class="html">
Don&apos;t forget, because $_REQUEST is a different variable than $_GET and $_POST, it is treated as such in PHP -- modifying $_GET or $_POST elements at runtime will not affect the ellements in $_REQUEST, nor vice versa.<br><br>e.g:<br><br><span class="default">&lt;?php<br><br>$_GET</span><span class="keyword">[</span><span class="string">&apos;foo&apos;</span><span class="keyword">] = </span><span class="string">&apos;a&apos;</span><span class="keyword">;<br></span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;bar&apos;</span><span class="keyword">] = </span><span class="string">&apos;b&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">); </span><span class="comment">// Element &apos;foo&apos; is string(1) &quot;a&quot;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">); </span><span class="comment">// Element &apos;bar&apos; is string(1) &quot;b&quot;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$_REQUEST</span><span class="keyword">); </span><span class="comment">// Does not contain elements &apos;foo&apos; or &apos;bar&apos;<br><br></span><span class="default">?&gt;<br></span><br>If you want to evaluate $_GET and $_POST variables by a single token without including $_COOKIE in the mix, use&#xA0; $_SERVER[&apos;REQUEST_METHOD&apos;] to identify the method used and set up a switch block accordingly, e.g:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">switch(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REQUEST_METHOD&apos;</span><span class="keyword">])<br>{<br>case </span><span class="string">&apos;GET&apos;</span><span class="keyword">: </span><span class="default">$the_request </span><span class="keyword">= &amp;</span><span class="default">$_GET</span><span class="keyword">; break;<br>case </span><span class="string">&apos;POST&apos;</span><span class="keyword">: </span><span class="default">$the_request </span><span class="keyword">= &amp;</span><span class="default">$_POST</span><span class="keyword">; break;<br>.<br>. </span><span class="comment">// Etc.<br></span><span class="keyword">.<br>default:<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.request.php)

**[⬆ to root](/)**