# set_include_path




<div class="phpcode"><span class="html">
If you find that this function is failing for you, and you&apos;re not sure why, you may have set your php include path in your sites&apos;s conf file in Apache&#xA0; (this may be true of .htaccess as well)<br><br>So to get it to work, comment out any &quot;php_value include_path&quot; type lines in your Apache conf file, and you should be able to set it now in your php code.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Can be useful to check the value of the constant PATH_SEPARATOR.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if ( ! </span><span class="default">defined</span><span class="keyword">( </span><span class="string">&quot;PATH_SEPARATOR&quot; </span><span class="keyword">) ) {
<br>&#xA0; if ( </span><span class="default">strpos</span><span class="keyword">( </span><span class="default">$_ENV</span><span class="keyword">[ </span><span class="string">&quot;OS&quot; </span><span class="keyword">], </span><span class="string">&quot;Win&quot; </span><span class="keyword">) !== </span><span class="default">false </span><span class="keyword">)
<br>&#xA0; &#xA0; </span><span class="default">define</span><span class="keyword">( </span><span class="string">&quot;PATH_SEPARATOR&quot;</span><span class="keyword">, </span><span class="string">&quot;;&quot; </span><span class="keyword">);
<br>&#xA0; else </span><span class="default">define</span><span class="keyword">( </span><span class="string">&quot;PATH_SEPARATOR&quot;</span><span class="keyword">, </span><span class="string">&quot;:&quot; </span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>For older versions of php, PATH_SEPARATOR is not defined.
<br>If it is so, we must check what kind of OS is on the web-server and define PATH_SEPARATOR properly</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.set-include-path.php)

**[To root](/README.md)**