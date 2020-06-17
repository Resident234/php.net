# Examples




<div class="phpcode"><span class="html">
Following code returns the curl output as a string.
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// create curl resource
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// set url
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="string">&quot;example.com&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//return the transfer as a string
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// $output contains the output string
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// close curl resource to free up system resources
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);&#xA0; &#xA0; &#xA0; 
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/curl.examples.php)

**[To root](/README.md)**