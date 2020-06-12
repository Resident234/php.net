# Something Useful




<div class="phpcode"><span class="html">
Please note that Internet Explorer 11 no longer contains MSIE in its user agent string, for example on Windows 8 with IE11 I get the following:<br><br>Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; rv:11.0) like Gecko<br><br>So if you want to include a test for IE11, the code above changes to: <br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">], </span><span class="string">&apos;MSIE&apos;</span><span class="keyword">) !== </span><span class="default">FALSE </span><span class="keyword">||<br>&#xA0; &#xA0; </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">], </span><span class="string">&apos;Trident&apos;</span><span class="keyword">) !== </span><span class="default">FALSE</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&apos;You are using Internet Explorer.&lt;br /&gt;&apos;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/tutorial.useful.php)

**[To root](/README.md)**