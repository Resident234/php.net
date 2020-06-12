# ob_get_clean




<div class="phpcode"><span class="html">
The definition should mention that the function also &quot;turns off output buffering&quot;, not just cleans it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Also, don&apos;t forget that you will need to ob_start() again for any successive calls:
<br>
<br><span class="default">&lt;?php
<br>ob_start</span><span class="keyword">();
<br>echo </span><span class="string">&quot;1&quot;</span><span class="keyword">;
<br></span><span class="default">$content </span><span class="keyword">= </span><span class="default">ob_get_clean</span><span class="keyword">();
<br>
<br></span><span class="default">ob_start</span><span class="keyword">(); </span><span class="comment">// This is NECESSARY for the next ob_get_clean() to work as intended.
<br></span><span class="keyword">echo </span><span class="string">&quot;2&quot;</span><span class="keyword">;
<br></span><span class="default">$content </span><span class="keyword">.= </span><span class="default">ob_get_clean</span><span class="keyword">();
<br>
<br>echo </span><span class="default">$content</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Output: 12
<br>
<br>Without the second ob_start(), the output is 21 ...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-get-clean.php)

**[⬆ to root](/)**