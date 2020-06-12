# curl_multi_init




<div class="phpcode"><span class="html">
Here&apos;s an easier to follow example, From : <a href="http://arguments.callee.info/2010/02/21/multiple-curl-requests-with-php/" rel="nofollow" target="_blank">http://arguments.callee.info/2010/02/21/multiple-curl-requests-with-php/</a><br><br>// build the individual requests, but do not execute them<br>$ch_1 = curl_init(&apos;<a href="http://webservice.one.com/" rel="nofollow" target="_blank">http://webservice.one.com/</a>&apos;);<br>$ch_2 = curl_init(&apos;<a href="http://webservice.two.com/" rel="nofollow" target="_blank">http://webservice.two.com/</a>&apos;);<br>curl_setopt($ch_1, CURLOPT_RETURNTRANSFER, true);<br>curl_setopt($ch_2, CURLOPT_RETURNTRANSFER, true);<br>&#xA0; <br>// build the multi-curl handle, adding both $ch<br>$mh = curl_multi_init();<br>curl_multi_add_handle($mh, $ch_1);<br>curl_multi_add_handle($mh, $ch_2);<br>&#xA0; <br>// execute all queries simultaneously, and continue when all are complete<br>&#xA0; $running = null;<br>&#xA0; do {<br>&#xA0; &#xA0; curl_multi_exec($mh, $running);<br>&#xA0; } while ($running);<br><br>//close the handles<br>curl_multi_remove_handle($mh, $ch1);<br>curl_multi_remove_handle($mh, $ch2);<br>curl_multi_close($mh);<br>&#xA0; <br>// all of our requests are done, we can now access the results<br>$response_1 = curl_multi_getcontent($ch_1);<br>$response_2 = curl_multi_getcontent($ch_2);<br>echo &quot;$response_1 $response_2&quot;; // output results</span>
</div>
  

#


<div class="phpcode"><span class="html">
According to <a href="https://bugs.php.net/bug.php?id=61141:" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=61141:</a><br><br>On Windows setups using libcurl version 7.24 or later (which seems to correspond to PHP 5.3.10 or later), you may find that curl_multi_select() always returns -1, causing the example code in the documentation to timeout. This is, apparently, not strictly a bug: according to the libcurl documentation, you should add your own sleep if curl_multi_select returns -1.<br><br>Therefore, in order to work correctly on Windows for PHP 5.3.10+, the second loop in the example code should look something like the following:<br><br><span class="default">&lt;?php<br></span><span class="keyword">while (</span><span class="default">$active </span><span class="keyword">&amp;&amp; </span><span class="default">$mrc </span><span class="keyword">== </span><span class="default">CURLM_OK</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">curl_multi_select</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">) == -</span><span class="default">1</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">usleep</span><span class="keyword">(</span><span class="default">100</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; do {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$mrc </span><span class="keyword">= </span><span class="default">curl_multi_exec</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">, </span><span class="default">$active</span><span class="keyword">);<br>&#xA0; &#xA0; } while (</span><span class="default">$mrc </span><span class="keyword">== </span><span class="default">CURLM_CALL_MULTI_PERFORM</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-init.php)

**[⬆ to root](/)**