# curl_multi_select




<div class="phpcode"><span class="html">
Even after so many distro releases, in PHP v5.5.9 and libcurl v7.35.0, curl_multi_select still returns -1. The only work around here is to wait and proceed like nothing ever happened. You have to determine the &quot;wait&quot; required here. <br><br>In my application a very small interval like usleep(1) worked. For example:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// While we&apos;re still active, execute curl<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$active </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; do {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$mrc </span><span class="keyword">= </span><span class="default">curl_multi_exec</span><span class="keyword">(</span><span class="default">$multi</span><span class="keyword">, </span><span class="default">$active</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; } while (</span><span class="default">$mrc </span><span class="keyword">== </span><span class="default">CURLM_CALL_MULTI_PERFORM</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; while (</span><span class="default">$active </span><span class="keyword">&amp;&amp; </span><span class="default">$mrc </span><span class="keyword">== </span><span class="default">CURLM_OK</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Wait for activity on any curl-connection<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">curl_multi_select</span><span class="keyword">(</span><span class="default">$multi</span><span class="keyword">) == -</span><span class="default">1</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">usleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Continue to exec until curl is ready to<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // give us more data<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">do {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$mrc </span><span class="keyword">= </span><span class="default">curl_multi_exec</span><span class="keyword">(</span><span class="default">$multi</span><span class="keyword">, </span><span class="default">$active</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } while (</span><span class="default">$mrc </span><span class="keyword">== </span><span class="default">CURLM_CALL_MULTI_PERFORM</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Internally php curl_multi_select uses libcurl curl_multi_fdset function and its libcurl documentation says :<br><a href="http://curl.haxx.se/libcurl/c/curl_multi_fdset.html" rel="nofollow" target="_blank">http://curl.haxx.se/libcurl/c/curl_multi_fdset.html</a><br><br>&quot;When libcurl returns -1 in max_fd, it is because libcurl currently does something that isn&apos;t possible for your application to monitor with a socket and unfortunately you can then not know exactly when the current action is completed using select(). When max_fd returns with -1, you need to wait a while and then proceed and call curl_multi_perform anyway. How long to wait? I would suggest 100 milliseconds at least, but you may want to test it out in your own particular conditions to find a suitable value. <br><br>When doing select(), you should use curl_multi_timeout to figure out how long to wait for action.&quot;<br><br>Untill PHP implements curl_multi_timeout() we have to play with our application and determine the &quot;wait&quot;.</span>
</div>
  

#


<div class="phpcode"><span class="html">
curl_multi_select($mh, $timeout) simply blocks for $timeout seconds while curl_multi_exec() returns CURLM_CALL_MULTI_PERFORM. Otherwise, it works as intended, and blocks until at least one connection has completed or $timeout seconds, whatever happens first.
<br>
<br>For that reason, curl_multi_exec() should always be wrapped:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; </span><span class="keyword">function </span><span class="default">full_curl_multi_exec</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">, &amp;</span><span class="default">$still_running</span><span class="keyword">) {
<br>&#xA0; &#xA0; do {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$rv </span><span class="keyword">= </span><span class="default">curl_multi_exec</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">, </span><span class="default">$still_running</span><span class="keyword">);
<br>&#xA0; &#xA0; } while (</span><span class="default">$rv </span><span class="keyword">== </span><span class="default">CURLM_CALL_MULTI_PERFORM</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">$rv</span><span class="keyword">;
<br>&#xA0; }
<br></span><span class="default">?&gt;
<br></span>
<br>With that, the core of &quot;multi&quot; processing becomes (ignoring error handling for brevity):
<br>
<br><span class="default">&lt;?php
<br>&#xA0; full_curl_multi_exec</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">, </span><span class="default">$still_running</span><span class="keyword">); </span><span class="comment">// start requests
<br>&#xA0; </span><span class="keyword">do { </span><span class="comment">// &quot;wait for completion&quot;-loop
<br>&#xA0; &#xA0; </span><span class="default">curl_multi_select</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">); </span><span class="comment">// non-busy (!) wait for state change
<br>&#xA0; &#xA0; </span><span class="default">full_curl_multi_exec</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">, </span><span class="default">$still_running</span><span class="keyword">); </span><span class="comment">// get new state
<br>&#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$info </span><span class="keyword">= </span><span class="default">curl_multi_info_read</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// process completed request (e.g. curl_multi_getcontent($info[&apos;handle&apos;]))
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; } while (</span><span class="default">$still_running</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Note that after starting requests, retrieval is done in the background - one of the better shots at parallel processing in PHP.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function blocks the calling process until there is activity on any of the connections opened by the curl_multi interface, or until the timeout period has expired. <br>In other words, it waits for data to be received in the opened connections.<br><br>Internally it fetches socket pointers with &quot;curl_multi_fdset()&quot; and runs the &quot;select()&quot; C function.<br>It returns in 3 cases:<br>1. Activity is detected on any socket;<br>2. Timeout has ended (second parameter);<br>3. Process received any signal (#man kill).<br><br>The function returns an integer:<br>* In case of activity it returns a number, usually 1.<br>I suppose, it returns the number of connections with activity detected.<br>* If timeout expires it returns 0<br>* In case of error it returns -1<br><br>Thanks for attention, hope this helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-select.php)

**[To root](/README.md)**