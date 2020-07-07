# strcmp




<div class="phpcode"><span class="html">
If you rely on strcmp for safe string comparisons, both parameters must be strings, the result is otherwise extremely unpredictable.<br>For instance you may get an unexpected 0, or return values of NULL, -2, 2, 3 and -3.<br><br>strcmp(&quot;5&quot;, 5) =&gt; 0<br>strcmp(&quot;15&quot;, 0xf) =&gt; 0<br>strcmp(61529519452809720693702583126814, 61529519452809720000000000000000) =&gt; 0<br>strcmp(NULL, false) =&gt; 0<br>strcmp(NULL, &quot;&quot;) =&gt; 0<br>strcmp(NULL, 0) =&gt; -1<br>strcmp(false, -1) =&gt; -2<br>strcmp(&quot;15&quot;, NULL) =&gt; 2<br>strcmp(NULL, &quot;foo&quot;) =&gt; -3<br>strcmp(&quot;foo&quot;, NULL) =&gt; 3<br>strcmp(&quot;foo&quot;, false) =&gt; 3<br>strcmp(&quot;foo&quot;, 0) =&gt; 1<br>strcmp(&quot;foo&quot;, 5) =&gt; 1<br>strcmp(&quot;foo&quot;, array()) =&gt; NULL + PHP Warning<br>strcmp(&quot;foo&quot;, new stdClass) =&gt; NULL + PHP Warning<br>strcmp(function(){}, &quot;&quot;) =&gt; NULL + PHP Warning</span>
</div>
  

#


<div class="phpcode"><span class="html">
i hope this will give you a clear idea how strcmp works internally.
<br>
<br><span class="default">&lt;?php
<br>$str1 </span><span class="keyword">= </span><span class="string">&quot;b&quot;</span><span class="keyword">;
<br>echo </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">); </span><span class="comment">//98
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br></span><span class="default">$str2 </span><span class="keyword">= </span><span class="string">&quot;t&quot;</span><span class="keyword">;
<br>echo </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">); </span><span class="comment">//116
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">)-</span><span class="default">ord</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">);</span><span class="comment">//-18
<br></span><span class="default">$str1 </span><span class="keyword">= </span><span class="string">&quot;bear&quot;</span><span class="keyword">;
<br></span><span class="default">$str2 </span><span class="keyword">= </span><span class="string">&quot;tear&quot;</span><span class="keyword">;
<br></span><span class="default">$str3 </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;&lt;pre&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">); </span><span class="comment">// -18
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">, </span><span class="default">$str1</span><span class="keyword">); </span><span class="comment">//18
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">); </span><span class="comment">//0
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">, </span><span class="default">$str3</span><span class="keyword">); </span><span class="comment">//4
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$str3</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">); </span><span class="comment">//-4
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$str3</span><span class="keyword">, </span><span class="default">$str3</span><span class="keyword">); </span><span class="comment">// 0
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;/pre&gt;&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
One big caveat - strings retrieved from the backtick operation may be zero terminated (C-style), and therefore will not be equal to the non-zero terminated strings (roughly Pascal-style) normal in PHP. The workaround is to surround every `` pair or shell_exec() function with the trim() function. This is likely to be an issue with other functions that invoke shells; I haven&apos;t bothered to check.<br><br>On Debian Lenny (and RHEL 5, with minor differences), I get this:<br><br>====PHP====<br><span class="default">&lt;?php<br>$sz </span><span class="keyword">= `</span><span class="string">pwd</span><span class="keyword">`;<br></span><span class="default">$ps </span><span class="keyword">= </span><span class="string">&quot;/var/www&quot;</span><span class="keyword">;<br><br>echo </span><span class="string">&quot;Zero-terminated string:&lt;br /&gt;sz = &quot;</span><span class="keyword">.</span><span class="default">$sz</span><span class="keyword">.</span><span class="string">&quot;&lt;br /&gt;str_split(sz) = &quot;</span><span class="keyword">; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">str_split</span><span class="keyword">(</span><span class="default">$sz</span><span class="keyword">));<br>echo </span><span class="string">&quot;&lt;br /&gt;&lt;br /&gt;&quot;</span><span class="keyword">;<br><br>echo </span><span class="string">&quot;Pascal-style string:&lt;br /&gt;ps = &quot;</span><span class="keyword">.</span><span class="default">$ps</span><span class="keyword">.</span><span class="string">&quot;&lt;br /&gt;str_split(ps) = &quot;</span><span class="keyword">; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">str_split</span><span class="keyword">(</span><span class="default">$ps</span><span class="keyword">));<br>echo </span><span class="string">&quot;&lt;br /&gt;&lt;br /&gt;&quot;</span><span class="keyword">;<br><br>echo </span><span class="string">&quot;Normal results of comparison:&lt;br /&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;sz == ps = &quot;</span><span class="keyword">.(</span><span class="default">$sz </span><span class="keyword">== </span><span class="default">$ps </span><span class="keyword">? </span><span class="string">&quot;true&quot; </span><span class="keyword">: </span><span class="string">&quot;false&quot;</span><span class="keyword">).</span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;strcmp(sz,ps) = &quot;</span><span class="keyword">.</span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$sz</span><span class="keyword">,</span><span class="default">$ps</span><span class="keyword">);<br>echo </span><span class="string">&quot;&lt;br /&gt;&lt;br /&gt;&quot;</span><span class="keyword">;<br><br>echo </span><span class="string">&quot;Comparison with trim()&apos;d zero-terminated string:&lt;br /&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;trim(sz) = &quot;</span><span class="keyword">.</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$sz</span><span class="keyword">).</span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;str_split(trim(sz)) = &quot;</span><span class="keyword">; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">str_split</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$sz</span><span class="keyword">))); echo </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;trim(sz) == ps = &quot;</span><span class="keyword">.(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$sz</span><span class="keyword">) == </span><span class="default">$ps </span><span class="keyword">? </span><span class="string">&quot;true&quot; </span><span class="keyword">: </span><span class="string">&quot;false&quot;</span><span class="keyword">).</span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;strcmp(trim(sz),ps) = &quot;</span><span class="keyword">.</span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$sz</span><span class="keyword">),</span><span class="default">$ps</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>====Output====<br>Zero-terminated string:<br>sz = /var/www <br>str_split(sz) = Array ( [0] =&gt; / [1] =&gt; v [2] =&gt; a [3] =&gt; r [4] =&gt; / [5] =&gt; w [6] =&gt; w [7] =&gt; w [8] =&gt; ) <br><br>Pascal-style string:<br>ps = /var/www<br>str_split(ps) = Array ( [0] =&gt; / [1] =&gt; v [2] =&gt; a [3] =&gt; r [4] =&gt; / [5] =&gt; w [6] =&gt; w [7] =&gt; w ) <br><br>Normal results of comparison:<br>sz == ps = false<br>strcmp(sz,ps) = 1<br><br>Comparison with trim()&apos;d zero-terminated string:<br>trim(sz) = /var/www<br>str_split(trim(sz)) = Array ( [0] =&gt; / [1] =&gt; v [2] =&gt; a [3] =&gt; r [4] =&gt; / [5] =&gt; w [6] =&gt; w [7] =&gt; w ) <br>trim(sz) == ps = true<br>strcmp(trim(sz),ps) = 0</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hey be sure the string you are comparing has not special characters like &apos;\n&apos; or something like that.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strcmp.php)

**[To root](/README.md)**