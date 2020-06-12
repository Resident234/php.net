# header




<div class="phpcode"><span class="html">
I strongly recommend, that you use
<br>
<br>header($_SERVER[&quot;SERVER_PROTOCOL&quot;].&quot; 404 Not Found&quot;);
<br>
<br>instead of 
<br>
<br>header(&quot;HTTP/1.1 404 Not Found&quot;);
<br>
<br>I had big troubles with an Apache/2.0.59 (Unix) answering in HTTP/1.0 while I (accidentially) added a &quot;HTTP/1.1 200 Ok&quot; - Header.
<br>
<br>Most of the pages were displayed correct, but on some of them apache added weird content to it:
<br>
<br>A 4-digits HexCode on top of the page (before any output of my php script), seems to be some kind of checksum, because it changes from page to page and browser to browser. (same code for same page and browser)
<br>
<br>&quot;0&quot; at the bottom of the page (after the complete output of my php script) 
<br>
<br>It took me quite a while to find out about the wrong protocol in the HTTP-header.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Several times this one is asked on the net but an answer could not be found in the docs on php.net ...
<br>
<br>If you want to redirect an user and tell him he will be redirected, e. g. &quot;You will be redirected in about 5 secs. If not, click here.&quot; you cannot use header( &apos;Location: ...&apos; ) as you can&apos;t sent any output before the headers are sent.
<br>
<br>So, either you have to use the HTML meta refresh thingy or you use the following:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; header</span><span class="keyword">( </span><span class="string">&quot;refresh:5;url=wherever.php&quot; </span><span class="keyword">);
<br>&#xA0; echo </span><span class="string">&apos;You\&apos;ll be redirected in about 5 secs. If not, click &lt;a href=&quot;wherever.php&quot;&gt;here&lt;/a&gt;.&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Hth someone</span>
</div>
  

#


<div class="phpcode"><span class="html">
A quick way to make redirects permanent or temporary is to make use of the $http_response_code parameter in header().<br><br><span class="default">&lt;?php<br></span><span class="comment">// 301 Moved Permanently<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Location: /foo.php&quot;</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">,</span><span class="default">301</span><span class="keyword">);<br><br></span><span class="comment">// 302 Found<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Location: /foo.php&quot;</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">,</span><span class="default">302</span><span class="keyword">);<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Location: /foo.php&quot;</span><span class="keyword">);<br><br></span><span class="comment">// 303 See Other<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Location: /foo.php&quot;</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">,</span><span class="default">303</span><span class="keyword">);<br><br></span><span class="comment">// 307 Temporary Redirect<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Location: /foo.php&quot;</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">,</span><span class="default">307</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The HTTP status code changes the way browsers and robots handle redirects, so if you are using header(Location:) it&apos;s a good idea to set the status code at the same time.&#xA0; Browsers typically re-request a 307 page every time, cache a 302 page for the session, and cache a 301 page for longer, or even indefinitely.&#xA0; Search engines typically transfer &quot;page rank&quot; to the new location for 301 redirects, but not for 302, 303 or 307. If the status code is not specified, header(&apos;Location:&apos;) defaults to 302.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When using PHP to output an image, it won&apos;t be cached by the client so if you don&apos;t want them to download the image each time they reload the page, you will need to emulate part of the HTTP protocol.<br><br>Here&apos;s how:<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="comment">// Test image.<br>&#xA0; &#xA0; </span><span class="default">$fn </span><span class="keyword">= </span><span class="string">&apos;/test/foo.png&apos;</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment">// Getting headers sent by the client.<br>&#xA0; &#xA0; </span><span class="default">$headers </span><span class="keyword">= </span><span class="default">apache_request_headers</span><span class="keyword">(); <br><br>&#xA0; &#xA0; </span><span class="comment">// Checking if the client is validating his cache and if it is current.<br>&#xA0; &#xA0; </span><span class="keyword">if (isset(</span><span class="default">$headers</span><span class="keyword">[</span><span class="string">&apos;If-Modified-Since&apos;</span><span class="keyword">]) &amp;&amp; (</span><span class="default">strtotime</span><span class="keyword">(</span><span class="default">$headers</span><span class="keyword">[</span><span class="string">&apos;If-Modified-Since&apos;</span><span class="keyword">]) == </span><span class="default">filemtime</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">))) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Client&apos;s cache IS current, so we just respond &apos;304 Not Modified&apos;.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Last-Modified: &apos;</span><span class="keyword">.</span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&apos;D, d M Y H:i:s&apos;</span><span class="keyword">, </span><span class="default">filemtime</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">)).</span><span class="string">&apos; GMT&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">304</span><span class="keyword">);<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Image not cached or cache outdated, we respond &apos;200 OK&apos; and output the image.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Last-Modified: &apos;</span><span class="keyword">.</span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&apos;D, d M Y H:i:s&apos;</span><span class="keyword">, </span><span class="default">filemtime</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">)).</span><span class="string">&apos; GMT&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">200</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Length: &apos;</span><span class="keyword">.</span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Type: image/png&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br></span><span class="default">?&gt;<br></span><br>That way foo.png will be properly cached by the client and you&apos;ll save bandwith. :)</span>
</div>
  

#


<div class="phpcode"><span class="html">
If using the &apos;header&apos; function for the downloading of files, especially if you&apos;re passing the filename as a variable, remember to surround the filename with double quotes, otherwise you&apos;ll have problems in Firefox as soon as there&apos;s a space in the filename.<br><br>So instead of typing:<br><br><span class="default">&lt;?php<br>&#xA0; header</span><span class="keyword">(</span><span class="string">&quot;Content-Disposition: attachment; filename=&quot; </span><span class="keyword">. </span><span class="default">basename</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>you should type:<br><br><span class="default">&lt;?php<br>&#xA0; header</span><span class="keyword">(</span><span class="string">&quot;Content-Disposition: attachment; filename=\&quot;&quot; </span><span class="keyword">. </span><span class="default">basename</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">) . </span><span class="string">&quot;\&quot;&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>If you don&apos;t do this then when the user clicks on the link for a file named &quot;Example file with spaces.txt&quot;, then Firefox&apos;s Save As dialog box will give it the name &quot;Example&quot;, and it will have no extension.<br><br>See the page called &quot;Filenames_with_spaces_are_truncated_upon_download&quot; at <br><a href="http://kb.mozillazine.org/" rel="nofollow" target="_blank">http://kb.mozillazine.org/</a> for more information. (Sorry, the site won&apos;t let me post such a long link...)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be aware that sending binary files to the user-agent (browser) over an encrypted connection (SSL/TLS) will fail in IE (Internet Explorer) versions 5, 6, 7, and 8 if any of the following headers is included:<br><br>Cache-control:no-store<br>Cache-control:no-cache<br><br>See: <a href="http://support.microsoft.com/kb/323308" rel="nofollow" target="_blank">http://support.microsoft.com/kb/323308</a><br><br>Workaround: do not send those headers.<br><br>Also, be aware that IE versions 5, 6, 7, and 8 double-compress already-compressed files and do not reverse the process correctly, so ZIP files and similar are corrupted on download.<br><br>Workaround: disable compression (beyond text/html) for these particular versions of IE, e.g., using Apache&apos;s &quot;BrowserMatch&quot; directive. The following example disables compression in all versions of IE:<br><br>BrowserMatch &quot;.*MSIE.*&quot; gzip-only-text/html</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.header.php)

**[⬆ to root](/)**