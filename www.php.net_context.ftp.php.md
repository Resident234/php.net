# FTP context options




<div class="phpcode"><span class="html">
This is an example of how to allow fopen() to overwrite a file on an FTP site. If the stream context is not modified, an error will occur: &quot;...failed to open stream: Remote file already exists and overwrite context option not specified...&quot;.<br><br><span class="default">&lt;?php<br></span><span class="comment">// The path to the FTP file, including login arguments<br></span><span class="default">$ftp_path </span><span class="keyword">= </span><span class="string">&apos;<a href="ftp://username:password@example.com/example.txt" rel="nofollow" target="_blank">ftp://username:password@example.com/example.txt</a>&apos;</span><span class="keyword">;<br><br></span><span class="comment">// Allows overwriting of existing files on the remote FTP server<br></span><span class="default">$stream_options </span><span class="keyword">= array(</span><span class="string">&apos;ftp&apos; </span><span class="keyword">=&gt; array(</span><span class="string">&apos;overwrite&apos; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">));<br><br></span><span class="comment">// Creates a stream context resource with the defined options<br></span><span class="default">$stream_context </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$stream_options</span><span class="keyword">);<br><br></span><span class="comment">// Opens the file for writing and truncates it to zero length <br></span><span class="keyword">if (</span><span class="default">$fh </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$ftp_path</span><span class="keyword">, </span><span class="string">&apos;w&apos;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$stream_context</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; </span><span class="comment">// Writes contents to the file<br>&#xA0; &#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, </span><span class="string">&apos;example contents&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// Closes the file handle<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">);<br>}<br>else<br>{<br>&#xA0; &#xA0; die(</span><span class="string">&apos;Could not open file.&apos;</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/context.ftp.php)

**[To root](/)**