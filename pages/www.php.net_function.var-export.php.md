# var_export




<div class="phpcode"><span class="html">
/**<br> * var_export() with square brackets and indented 4 spaces.<br> */<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">varexport</span><span class="keyword">(</span><span class="default">$expression</span><span class="keyword">, </span><span class="default">$return</span><span class="keyword">=</span><span class="default">FALSE</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$export </span><span class="keyword">= </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$expression</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$export </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&quot;/^([ ]*)(.*)/m&quot;</span><span class="keyword">, </span><span class="string">&apos;$1$1$2&apos;</span><span class="keyword">, </span><span class="default">$export</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&quot;/\r\n|\n|\r/&quot;</span><span class="keyword">, </span><span class="default">$export</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">([</span><span class="string">&quot;/\s*array\s\($/&quot;</span><span class="keyword">, </span><span class="string">&quot;/\)(,)?$/&quot;</span><span class="keyword">, </span><span class="string">&quot;/\s=&gt;\s$/&quot;</span><span class="keyword">], [</span><span class="default">NULL</span><span class="keyword">, </span><span class="string">&apos;]$1&apos;</span><span class="keyword">, </span><span class="string">&apos; =&gt; [&apos;</span><span class="keyword">], </span><span class="default">$array</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$export </span><span class="keyword">= </span><span class="default">join</span><span class="keyword">(</span><span class="default">PHP_EOL</span><span class="keyword">, </span><span class="default">array_filter</span><span class="keyword">([</span><span class="string">&quot;[&quot;</span><span class="keyword">] + </span><span class="default">$array</span><span class="keyword">));<br>&#xA0; &#xA0; if ((bool)</span><span class="default">$return</span><span class="keyword">) return </span><span class="default">$export</span><span class="keyword">; else echo </span><span class="default">$export</span><span class="keyword">;<br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Looks like since version 5.4.22 var_export uses the serialize_precision ini setting, rather than the precision one used for normal output of floating-point numbers.<br>As a consequence since version 5.4.22 for example var_export(1.1) will output 1.1000000000000001 (17 is default precision value) and not 1.1 as before. <br><br><span class="default">&lt;?php <br></span><span class="comment">//ouput 1.1000000000000001<br></span><span class="default">var_export</span><span class="keyword">(</span><span class="default">1.1</span><span class="keyword">)<br> </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It doesn&apos;t appear to be documented, but the behaviour of `var_export()` changed in PHP 7.<br><br>Previously, `var_export(3.)` returned &quot;3&quot;, now it returns &quot;3.0&quot;.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I found that my complex type was exporting with <br>&#xA0; stdClass::__set_state()<br>in places. Not only was that strange and messy, it cannot be eval()-ed back in at all. Fatal error. Doh!<br><br>However a quick string-replace tidy-up of the result rendered it valid again.<br><br>&#xA0; &#xA0; $macro = var_export($data, TRUE);<br>&#xA0; &#xA0; $macro = str_replace(&quot;stdClass::__set_state&quot;, &quot;(object)&quot;, $macro);<br>&#xA0; &#xA0; $macro = &apos;$data = &apos; . $macro . &apos;;&apos;;<br><br>And now the string I output *can* be evaluated back in again.</span>
</div>
  

#


<div class="phpcode"><span class="html">
&lt;roman at DIESPAM dot feather dot org dot ru&gt;, your function has inefficiencies and problems. I probably speak for everyone when I ask you to test code before you add to the manual.<br><br>Since the issue of whitespace only comes up when exporting arrays, you can use the original var_export() for all other variable types. This function does the job, and, from the outside, works the same as var_export().<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">var_export_min</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">, </span><span class="default">$return </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$toImplode </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$var </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$toImplode</span><span class="keyword">[] = </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">).</span><span class="string">&apos;=&gt;&apos;</span><span class="keyword">.</span><span class="default">var_export_min</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$code </span><span class="keyword">= </span><span class="string">&apos;array(&apos;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$toImplode</span><span class="keyword">).</span><span class="string">&apos;)&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$return</span><span class="keyword">) return </span><span class="default">$code</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else echo </span><span class="default">$code</span><span class="keyword">;<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">, </span><span class="default">$return</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.var-export.php)

**[To root](/README.md)**