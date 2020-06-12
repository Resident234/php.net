# str_replace




<div class="phpcode"><span class="html">
A faster way to replace the strings in multidimensional array is to json_encode() it, do the str_replace() and then json_decode() it, like this:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">str_replace_json</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$replace</span><span class="keyword">, </span><span class="default">$subject</span><span class="keyword">){
<br>&#xA0; &#xA0;&#xA0; return </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$replace</span><span class="keyword">,&#xA0; </span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$subject</span><span class="keyword">)));
<br>
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>This method is almost 3x faster (in 10000 runs.) than using recursive calling and looping method, and 10x simpler in coding.
<br>
<br>Compared to:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">str_replace_deep</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$replace</span><span class="keyword">, </span><span class="default">$subject</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$subject</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$subject </span><span class="keyword">as &amp;</span><span class="default">$oneSubject</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$oneSubject </span><span class="keyword">= </span><span class="default">str_replace_deep</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$replace</span><span class="keyword">, </span><span class="default">$oneSubject</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$oneSubject</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$subject</span><span class="keyword">;
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$replace</span><span class="keyword">, </span><span class="default">$subject</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that this does not replace strings that become part of replacement strings. This may be a problem when you want to remove multiple instances of the same repetative pattern, several times in a row.
<br>
<br>If you want to remove all dashes but one from the string &apos;-aaa----b-c-----d--e---f&apos; resulting in &apos;-aaa-b-c-d-e-f&apos;, you cannot use str_replace. Instead, use preg_replace:
<br>
<br><span class="default">&lt;?php
<br>$challenge </span><span class="keyword">= </span><span class="string">&apos;-aaa----b-c-----d--e---f&apos;</span><span class="keyword">;
<br>echo </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;--&apos;</span><span class="keyword">, </span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="default">$challenge</span><span class="keyword">).</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>echo </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/--+/&apos;</span><span class="keyword">, </span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="default">$challenge</span><span class="keyword">).</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>This outputs the following:
<br>-aaa--b-c---d-e--f
<br>-aaa-b-c-d-e-f</span>
</div>
  

#


<div class="phpcode"><span class="html">
Feel free to optimize this using the while/for or anything else, but this is a bit of code that allows you to replace strings found in an associative array.
<br>
<br>For example:
<br><span class="default">&lt;?php
<br>$replace </span><span class="keyword">= array(
<br></span><span class="string">&apos;dog&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;cat&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;apple&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;orange&apos;
<br>&apos;chevy&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;ford&apos;
<br></span><span class="keyword">);
<br>
<br></span><span class="default">$string </span><span class="keyword">= </span><span class="string">&apos;I like to eat an apple with my dog in my chevy&apos;</span><span class="keyword">;
<br>
<br>echo </span><span class="default">str_replace_assoc</span><span class="keyword">(</span><span class="default">$replace</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);
<br>
<br></span><span class="comment">// Echo: I like to eat an orange with my cat in my ford
<br></span><span class="default">?&gt;
<br></span>
<br>Here is the function:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">strReplaceAssoc</span><span class="keyword">(array </span><span class="default">$replace</span><span class="keyword">, </span><span class="default">$subject</span><span class="keyword">) {
<br>&#xA0;&#xA0; return </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$replace</span><span class="keyword">), </span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$replace</span><span class="keyword">), </span><span class="default">$subject</span><span class="keyword">);&#xA0; &#xA0; 
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>[Jun 1st, 2010 - EDIT BY thiago AT php DOT net: Function has been replaced with an updated version sent by ljelinek AT gmail DOT com]</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful when replacing characters (or repeated patterns in the FROM and TO arrays):
<br>
<br>For example:
<br>
<br><span class="default">&lt;?php
<br>$arrFrom </span><span class="keyword">= array(</span><span class="string">&quot;1&quot;</span><span class="keyword">,</span><span class="string">&quot;2&quot;</span><span class="keyword">,</span><span class="string">&quot;3&quot;</span><span class="keyword">,</span><span class="string">&quot;B&quot;</span><span class="keyword">);
<br></span><span class="default">$arrTo </span><span class="keyword">= array(</span><span class="string">&quot;A&quot;</span><span class="keyword">,</span><span class="string">&quot;B&quot;</span><span class="keyword">,</span><span class="string">&quot;C&quot;</span><span class="keyword">,</span><span class="string">&quot;D&quot;</span><span class="keyword">);
<br></span><span class="default">$word </span><span class="keyword">= </span><span class="string">&quot;ZBB2&quot;</span><span class="keyword">;
<br>echo </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$arrFrom</span><span class="keyword">, </span><span class="default">$arrTo</span><span class="keyword">, </span><span class="default">$word</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>I would expect as result: &quot;ZDDB&quot;
<br>However, this return: &quot;ZDDD&quot;
<br>(Because B = D according to our array)
<br>
<br>To make this work, use &quot;strtr&quot; instead:
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= array(</span><span class="string">&quot;1&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;A&quot;</span><span class="keyword">,</span><span class="string">&quot;2&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;B&quot;</span><span class="keyword">,</span><span class="string">&quot;3&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;C&quot;</span><span class="keyword">,</span><span class="string">&quot;B&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;D&quot;</span><span class="keyword">);
<br></span><span class="default">$word </span><span class="keyword">= </span><span class="string">&quot;ZBB2&quot;</span><span class="keyword">;
<br>echo </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">,</span><span class="default">$arr</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>This returns: &quot;ZDDB&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be aware that if you use this for filtering &amp; sanitizing some form of user input, or remove ALL instances of a string, there&apos;s another gotcha to watch out for:<br><br>// Remove all double characters<br>$string=&quot;1001011010&quot;;<br>$string=str_replace(array(&quot;11&quot;,&quot;00&quot;),&quot;&quot;,$string);<br>// Output: &quot;110010&quot;<br><br>$string=&quot;&lt;ht&lt;html&gt;ml&gt; Malicious code &lt;/&lt;html&gt;html&gt; etc&quot;;<br>$string=str_replace(array(&quot;&lt;html&gt;&quot;,&quot;&lt;/html&gt;&quot;),&quot;&quot;,$string);<br>// Output: &quot;&lt;html&gt; Malicious code &lt;/html&gt; etc&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-replace.php)

**[⬆ to root](/)**