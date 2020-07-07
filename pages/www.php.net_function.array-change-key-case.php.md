# array_change_key_case




<div class="phpcode"><span class="html">
Here is the most compact way to lower case keys in a multidimensional array<br><br>function array_change_key_case_recursive($arr)<br>{<br>&#xA0; &#xA0; return array_map(function($item){<br>&#xA0; &#xA0; &#xA0; &#xA0; if(is_array($item))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $item = array_change_key_case_recursive($item);<br>&#xA0; &#xA0; &#xA0; &#xA0; return $item;<br>&#xA0; &#xA0; },array_change_key_case($arr));<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Unicode example;<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_change_key_case_unicode</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">$c </span><span class="keyword">= </span><span class="default">CASE_LOWER</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= (</span><span class="default">$c </span><span class="keyword">== </span><span class="default">CASE_LOWER</span><span class="keyword">) ? </span><span class="default">MB_CASE_LOWER </span><span class="keyword">: </span><span class="default">MB_CASE_UPPER</span><span class="keyword">;<br>&#xA0; &#xA0; foreach (</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ret</span><span class="keyword">[</span><span class="default">mb_convert_case</span><span class="keyword">(</span><span class="default">$k</span><span class="keyword">, </span><span class="default">$c</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">)] = </span><span class="default">$v</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$ret</span><span class="keyword">;<br>}<br><br></span><span class="default">$arr </span><span class="keyword">= array(</span><span class="string">&quot;FirSt&quot; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&quot;ya&#x11F;&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;Oil&quot;</span><span class="keyword">, </span><span class="string">&quot;&#x15F;ekER&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;sugar&quot;</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_change_key_case</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">CASE_UPPER</span><span class="keyword">));<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_change_key_case_unicode</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">CASE_UPPER</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Array<br>(<br>&#xA0; &#xA0; [FIRST] =&gt; 1<br>&#xA0; &#xA0; [YA&#x11F;] =&gt; Oil<br>&#xA0; &#xA0; [&#x15F;EKER] =&gt; sugar<br>)<br>Array<br>(<br>&#xA0; &#xA0; [FIRST] =&gt; 1<br>&#xA0; &#xA0; [YA&#x11E;] =&gt; Oil<br>&#xA0; &#xA0; [&#x15E;EKER] =&gt; sugar<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-change-key-case.php)

**[To root](/README.md)**