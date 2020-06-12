# get_headers




<div class="phpcode"><span class="html">
Seems like there are some people who are looking for only the 3-digit HTTP response code&#xA0; - here is a quick and nasty solution:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">get_http_response_code</span><span class="keyword">(</span><span class="default">$theURL</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$headers </span><span class="keyword">= </span><span class="default">get_headers</span><span class="keyword">(</span><span class="default">$theURL</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$headers</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">9</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>How easy is that? Echo the function containing the URL you want to check the response code for, and voil&#xE0;. Custom redirects, alternative for blocked is_file() or flie_exists() functions (like I seem to have on my servers) hence the cheap workaround. But hey - it works!<br><br>Pudding</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-headers.php)

**[⬆ to root](/)**