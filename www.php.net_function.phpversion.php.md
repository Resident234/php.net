# phpversion




<div class="phpcode"><span class="html">
If you&apos;re trying to check whether the version of PHP you&apos;re running on is sufficient, don&apos;t screw around with `strcasecmp` etc.&#xA0; PHP already has a `version_compare` function, and it&apos;s specifically made to compare PHP-style version strings.<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&apos;5.3.10&apos;</span><span class="keyword">, </span><span class="string">&apos;&lt;&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="comment">// php version isn&apos;t high enough<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that the version string returned by phpversion() may include more information than expected: &quot;5.5.9-1ubuntu4.17&quot;, for example.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To know, what are the {php} extensions loaded &amp; version of extensions :
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">foreach (</span><span class="default">get_loaded_extensions</span><span class="keyword">() as </span><span class="default">$i </span><span class="keyword">=&gt; </span><span class="default">$ext</span><span class="keyword">)
<br>{
<br>&#xA0;&#xA0; echo </span><span class="default">$ext </span><span class="keyword">.</span><span class="string">&apos; =&gt; &apos;</span><span class="keyword">. </span><span class="default">phpversion</span><span class="keyword">(</span><span class="default">$ext</span><span class="keyword">). </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.phpversion.php)

**[To root](/)**