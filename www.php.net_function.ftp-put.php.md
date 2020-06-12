# ftp_put




<div class="phpcode"><span class="html">
ftp_put() overwrites existing files. <br>Again: trivial but not mentioned here.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If when using ftp_put you get the one of the following errors:<br><br>Warning: ftp_put() [function.ftp-put]: Opening ASCII mode data connection<br><br>Warning: ftp_put() [function.ftp-put]: Opening BINARY mode data connection<br><br>and it creates the file in the correct location but is a 0kb file and all FTP commands thereafter fail. It is likely that the client is behind a firewall. To rectify this use:<br><br><span class="default">&lt;?php<br>ftp_pasv</span><span class="keyword">(</span><span class="default">$resource</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Before executing any put commands. Took me so long to figure this out I actually cheered when I did :D</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to copy a whole directory tree (with subdiretories), <br>this function (ftp_copy) might be usefull. Tested with <br>php 4.2.2 and a Linux OS. <br><br>Example:<br>----------------------------------------------------------------<br>$conn_id = ftp_connect(&quot;server_adress&quot;); <br>...<br><br>$src_dir = &quot;/from&quot;;<br>$dst_dir = &quot;/to&quot;;<br><br>ftp_copy($src_dir, $dst_dir);<br>...<br>ftp_close($conn_id)<br><br>Function: ftp_copy()<br>----------------------------------------------------------------<br>function ftp_copy($src_dir, $dst_dir) {<br><br>global $conn_id;<br><br>$d = dir($src_dir);<br><br>&#xA0; &#xA0; while($file = $d-&gt;read()) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if ($file != &quot;.&quot; &amp;&amp; $file != &quot;..&quot;) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (is_dir($src_dir.&quot;/&quot;.$file)) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!@ftp_chdir($conn_id, $dst_dir.&quot;/&quot;.$file)) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ftp_mkdir($conn_id, $dst_dir.&quot;/&quot;.$file);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ftp_copy($src_dir.&quot;/&quot;.$file, $dst_dir.&quot;/&quot;.$file);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $upload = ftp_put($conn_id, $dst_dir.&quot;/&quot;.$file, $src_dir.&quot;/&quot;.$file, FTP_BINARY);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>$d-&gt;close();<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Got this cryptic error
<br>
<br>Warning:&#xA0; ftp_put() [function.ftp-put]: &apos;STOR&apos; not understood in 
<br>C:\wamp\www\kevtest\ftp_todays.php on line 48
<br>
<br>Found the prob, you can&apos;t put a path to the destination file
<br>(even though I can do that in the dos ftp client...?)
<br>
<br>e.g. - this doesn&apos;t work
<br>ftp_put($conn, &apos;/www/site/file.html&apos;,&apos;c:/wamp/www/site/file.html&apos;,FTP_BINARY);
<br>
<br>you have to put
<br>
<br><span class="default">&lt;?php
<br>ftp_chdir</span><span class="keyword">(</span><span class="default">$conn</span><span class="keyword">, </span><span class="string">&apos;/www/site/&apos;</span><span class="keyword">);
<br></span><span class="default">ftp_put</span><span class="keyword">(</span><span class="default">$conn</span><span class="keyword">,</span><span class="string">&apos;file.html&apos;</span><span class="keyword">, </span><span class="string">&apos;c:/wamp/www/site/file.html&apos;</span><span class="keyword">, </span><span class="default">FTP_BINARY </span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-put.php)

**[⬆ to root](/)**