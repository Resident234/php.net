# DOMDocument::load




<div class="phpcode"><span class="html">
I had a problem with loading documents over HTTP. I would get errors looking like this:<br><br>Warning: DOMDocument::load(<a href="http://external/document.xml" rel="nofollow" target="_blank">http://external/document.xml</a>): failed to open stream: HTTP request failed! HTTP/1.1 500 Internal Server Error<br><br>The document would load fine in browsers and using wget. The problem is that DOMDocument::load() on my systems (both OS X and Linux) didn&apos;t send any User-Agent header which for some weird reason made Microsoft-IIS/6.0 respond with the 500 error.<br><br>The solution is found on <a href="http://php.net/manual/en/function.libxml-set-streams-context.php" rel="nofollow" target="_blank">http://php.net/manual/en/function.libxml-set-streams-context.php</a> : <br><br><span class="default">&lt;?php<br>$opts </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;http&apos; </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;user_agent&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;PHP libxml agent&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; )<br>);<br><br></span><span class="default">$context </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$opts</span><span class="keyword">);<br></span><span class="default">libxml_set_streams_context</span><span class="keyword">(</span><span class="default">$context</span><span class="keyword">);<br><br></span><span class="comment">// request a file through HTTP<br></span><span class="default">$doc </span><span class="keyword">= </span><span class="default">DOMDocument</span><span class="keyword">::</span><span class="default">load</span><span class="keyword">(</span><span class="string">&apos;<a href="http://www.example.com/file.xml" rel="nofollow" target="_blank">http://www.example.com/file.xml</a>&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.load.php)

**[To root](/README.md)**