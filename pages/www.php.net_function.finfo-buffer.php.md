# finfo_buffer




<div class="phpcode"><span class="html">
You can easily check mime type of an internet resource using this code :<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">getUrlMimeType</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$buffer </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$finfo </span><span class="keyword">= new </span><span class="default">finfo</span><span class="keyword">(</span><span class="default">FILEINFO_MIME_TYPE</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$finfo</span><span class="keyword">-&gt;</span><span class="default">buffer</span><span class="keyword">(</span><span class="default">$buffer</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>I&apos;m using it to detect if an url given by a user is a HTML page (so I do some stuff with the HTML) or a file on Internet (so I show an icon accordingly to the mime type).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-buffer.php)

**[To root](/README.md)**