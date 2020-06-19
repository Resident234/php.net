# imap_savebody




<div class="phpcode"><span class="html">
By using imap_fetchbody() you may run in trouble by using too much memory. Using imap_savebody() may prevent this. 
<br>
<br>But the content will be encoded, in other words it is useless. Adding a filter can help here.
<br>
<br><span class="default">&lt;?php
<br>$whandle </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;./incomming/tmp.tif&apos;</span><span class="keyword">,</span><span class="string">&apos;w&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">stream_filter_append</span><span class="keyword">(</span><span class="default">$whandle</span><span class="keyword">, 
<br>&#xA0;&#xA0; </span><span class="string">&apos;convert.base64-decode&apos;</span><span class="keyword">,</span><span class="default">STREAM_FILTER_WRITE</span><span class="keyword">);
<br>
<br></span><span class="default">imap_savebody </span><span class="keyword">(</span><span class="default">$mbox</span><span class="keyword">, </span><span class="default">$whandle</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="default">$partcounter</span><span class="keyword">++);
<br>
<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$whandle</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>NOTE: To find the proper filter you need to check the encoding given by the structure of the body.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-savebody.php)

**[To root](/README.md)**