# HTTP context options




<div class="phpcode"><span class="html">
Note that if you set the protocol_version option to 1.1 and the server you are requesting from is configured to use keep-alive connections, the function (fopen, file_get_contents, etc.) will &quot;be slow&quot; and take a long time to complete. This is a feature of the HTTP 1.1 protocol you are unlikely to use with stream contexts in PHP.<br><br>Simply add a &quot;Connection: close&quot; header to the request to eliminate the keep-alive timeout:<br><br><span class="default">&lt;?php<br></span><span class="comment">// php 5.4 : array syntax and header option with array value<br></span><span class="default">$data </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;<a href="http://www.example.com/" rel="nofollow" target="_blank">http://www.example.com/</a>&apos;</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="default">stream_context_create</span><span class="keyword">([<br>&#xA0; &#xA0; </span><span class="string">&apos;http&apos; </span><span class="keyword">=&gt; [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;protocol_version&apos; </span><span class="keyword">=&gt; </span><span class="default">1.1</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;header&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; [<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;Connection: close&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; ],<br>&#xA0; &#xA0; ],<br>]));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/context.http.php)

**[⬆ to root](/)**