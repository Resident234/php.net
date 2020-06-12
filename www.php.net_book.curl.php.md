# Client URL Library




<div class="phpcode"><span class="html">
I wrote the following to see if a submitted URL has a valid http response code and also if it responds quickly. 
<br>
<br>Use the code like this:
<br>
<br><span class="default">&lt;?php
<br>$is_ok </span><span class="keyword">= </span><span class="default">http_response</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">); </span><span class="comment">// returns true only if http response code &lt; 400
<br></span><span class="default">?&gt;
<br></span>
<br>The second argument is optional, and it allows you to check for&#xA0; a specific response code
<br>
<br><span class="default">&lt;?php
<br>http_response</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">,</span><span class="string">&apos;400&apos;</span><span class="keyword">); </span><span class="comment">// returns true if http status is 400
<br></span><span class="default">?&gt;
<br></span>
<br>The third allows you to specify how long you are willing to wait for a response.
<br>
<br><span class="default">&lt;?php
<br>http_response</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">,</span><span class="string">&apos;200&apos;</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">); </span><span class="comment">// returns true if the response takes less than 3 seconds and the response code is 200
<br></span><span class="default">?&gt;
<br></span>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">http_response</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="default">$status </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">, </span><span class="default">$wait </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$time </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$expire </span><span class="keyword">= </span><span class="default">$time </span><span class="keyword">+ </span><span class="default">$wait</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// we fork the process so we don&apos;t have to wait for a timeout
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$pid </span><span class="keyword">== -</span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die(</span><span class="string">&apos;could not fork&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else if (</span><span class="default">$pid</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// we are the parent
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$url</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_HEADER</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_NOBODY</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">); </span><span class="comment">// remove body
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$head </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$httpCode </span><span class="keyword">= </span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLINFO_HTTP_CODE</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$head</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$status </span><span class="keyword">=== </span><span class="default">null</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$httpCode </span><span class="keyword">&lt; </span><span class="default">400</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">TRUE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif(</span><span class="default">$status </span><span class="keyword">== </span><span class="default">$httpCode</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">TRUE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">pcntl_wait</span><span class="keyword">(</span><span class="default">$status</span><span class="keyword">); </span><span class="comment">//Protect against Zombie children
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">} else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// we are the child
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">while(</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">) &lt; </span><span class="default">$expire</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">0.5</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this example helps.&#xA0; It is not 100% tested, so any feedback [sent directly to me by email] is appreciated.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.curl.php)

**[⬆ to root](/)**