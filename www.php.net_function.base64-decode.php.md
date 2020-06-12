# base64_decode




<div class="phpcode"><span class="html">
If you want to save data that is derived from a Javascript canvas.toDataURL() function, you have to convert blanks into plusses. If you do not do that, the decoded data is corrupted:<br><br><span class="default">&lt;?php<br>&#xA0; $encodedData </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">,</span><span class="string">&apos;+&apos;</span><span class="keyword">,</span><span class="default">$encodedData</span><span class="keyword">);<br>&#xA0; </span><span class="default">$decocedData </span><span class="keyword">= </span><span class="default">base64_decode</span><span class="keyword">(</span><span class="default">$encodedData</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.base64-decode.php)

**[⬆ to root](/)**