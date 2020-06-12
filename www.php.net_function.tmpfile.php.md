# tmpfile




<div class="phpcode"><span class="html">
To get the underlying file path of a tmpfile file pointer:<br><br><span class="default">&lt;?php<br>$file </span><span class="keyword">= </span><span class="default">tmpfile</span><span class="keyword">();<br></span><span class="default">$path </span><span class="keyword">= </span><span class="default">stream_get_meta_data</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">)[</span><span class="string">&apos;uri&apos;</span><span class="keyword">]; </span><span class="comment">// eg: /tmp/phpFx0513a</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I found this function useful when uploading a file through FTP. One of the files I was uploading was input from a textarea on the previous page, so really there was no &quot;file&quot; to upload, this solved the problem nicely:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment"># Upload setup.inc<br>&#xA0; &#xA0; </span><span class="default">$fSetup </span><span class="keyword">= </span><span class="default">tmpfile</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fSetup</span><span class="keyword">,</span><span class="default">$setup</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$fSetup</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">);<br>&#xA0; &#xA0; if (!</span><span class="default">ftp_fput</span><span class="keyword">(</span><span class="default">$ftp</span><span class="keyword">,</span><span class="string">&quot;inc/setup.inc&quot;</span><span class="keyword">,</span><span class="default">$fSetup</span><span class="keyword">,</span><span class="default">FTP_ASCII</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;br /&gt;&lt;i&gt;Setup file NOT inserted&lt;/i&gt;&lt;br /&gt;&lt;br /&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fSetup</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The $setup variable is the contents of the textarea.<br><br>And I&apos;m not sure if you need the fseek($temp,0); in there either, just leave it unless you know it doesn&apos;t effect it.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.tmpfile.php)

**[⬆ to root](/)**