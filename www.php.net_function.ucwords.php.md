# ucwords




<div class="phpcode"><span class="html">
My quick and dirty ucname (Upper Case Name) function.<br><br><span class="default">&lt;?php<br></span><span class="comment">//FUNCTION<br><br></span><span class="keyword">function </span><span class="default">ucname</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">=</span><span class="default">ucwords</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">));<br><br>&#xA0; &#xA0; foreach (array(</span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="string">&apos;\&apos;&apos;</span><span class="keyword">) as </span><span class="default">$delimiter</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$delimiter</span><span class="keyword">)!==</span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">=</span><span class="default">implode</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;ucfirst&apos;</span><span class="keyword">, </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">)));<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$string</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br>&lt;?php<br></span><span class="comment">//TEST<br><br></span><span class="default">$names </span><span class="keyword">=array(<br>&#xA0; </span><span class="string">&apos;JEAN-LUC PICARD&apos;</span><span class="keyword">,<br>&#xA0; </span><span class="string">&apos;MILES O\&apos;BRIEN&apos;</span><span class="keyword">,<br>&#xA0; </span><span class="string">&apos;WILLIAM RIKER&apos;</span><span class="keyword">,<br>&#xA0; </span><span class="string">&apos;geordi la forge&apos;</span><span class="keyword">,<br>&#xA0; </span><span class="string">&apos;bEvErly CRuSHeR&apos;<br></span><span class="keyword">);<br>foreach (</span><span class="default">$names </span><span class="keyword">as </span><span class="default">$name</span><span class="keyword">) { print </span><span class="default">ucname</span><span class="keyword">(</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$name</span><span class="keyword">}</span><span class="string">\n&quot;</span><span class="keyword">); }<br><br></span><span class="comment">//PRINTS:<br>/*<br>Jean-Luc Picard<br>Miles O&apos;Brien<br>William Riker<br>Geordi La Forge<br>Beverly Crusher<br>*/<br></span><span class="default">?&gt;<br></span><br>You can add more delimiters in the for-each loop array if you want to handle more characters.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Para formatar nomes em pt-br:<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">titleCase</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$delimiters </span><span class="keyword">= array(</span><span class="string">&quot; &quot;</span><span class="keyword">, </span><span class="string">&quot;-&quot;</span><span class="keyword">, </span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="string">&quot;&apos;&quot;</span><span class="keyword">, </span><span class="string">&quot;O&apos;&quot;</span><span class="keyword">, </span><span class="string">&quot;Mc&quot;</span><span class="keyword">), </span><span class="default">$exceptions </span><span class="keyword">= array(</span><span class="string">&quot;de&quot;</span><span class="keyword">, </span><span class="string">&quot;da&quot;</span><span class="keyword">, </span><span class="string">&quot;dos&quot;</span><span class="keyword">, </span><span class="string">&quot;das&quot;</span><span class="keyword">, </span><span class="string">&quot;do&quot;</span><span class="keyword">, </span><span class="string">&quot;I&quot;</span><span class="keyword">, </span><span class="string">&quot;II&quot;</span><span class="keyword">, </span><span class="string">&quot;III&quot;</span><span class="keyword">, </span><span class="string">&quot;IV&quot;</span><span class="keyword">, </span><span class="string">&quot;V&quot;</span><span class="keyword">, </span><span class="string">&quot;VI&quot;</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/*<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * Exceptions in lower case are words you don&apos;t want converted<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * Exceptions all in upper case are any words you don&apos;t want converted to title case<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; *&#xA0;&#xA0; but should be converted to upper case, e.g.:<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; *&#xA0;&#xA0; king henry viii or king henry Viii should be King Henry VIII<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">mb_convert_case</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">MB_CASE_TITLE</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$delimiters </span><span class="keyword">as </span><span class="default">$dlnr </span><span class="keyword">=&gt; </span><span class="default">$delimiter</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$words </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newwords </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$words </span><span class="keyword">as </span><span class="default">$wordnr </span><span class="keyword">=&gt; </span><span class="default">$word</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">), </span><span class="default">$exceptions</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// check exceptions list for any words that should be in upper case<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$word </span><span class="keyword">= </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } elseif (</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">mb_strtolower</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">), </span><span class="default">$exceptions</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// check exceptions list for any words that should be in upper case<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$word </span><span class="keyword">= </span><span class="default">mb_strtolower</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } elseif (!</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="default">$exceptions</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// convert to uppercase (non-utf8 only)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$word </span><span class="keyword">= </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$newwords</span><span class="keyword">, </span><span class="default">$word</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">join</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$newwords</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0;&#xA0; }</span><span class="comment">//foreach<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">return </span><span class="default">$string</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br></span><span class="default">?&gt;<br></span><br>Usage:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $s </span><span class="keyword">= </span><span class="string">&apos;S&#xC3;O JO&#xC3;O DOS SANTOS&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$v </span><span class="keyword">= </span><span class="default">titleCase</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">); </span><span class="comment">// &apos;S&#xE3;o Jo&#xE3;o dos Santos&apos; <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some recipes for switching between underscore and camelcase naming:
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// underscored to upper-camelcase
<br>// e.g. &quot;this_method_name&quot; -&gt; &quot;ThisMethodName&quot;
<br></span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/(?:^|_)(.?)/e&apos;</span><span class="keyword">,</span><span class="string">&quot;strtoupper(&apos;$1&apos;)&quot;</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);
<br>
<br></span><span class="comment">// underscored to lower-camelcase
<br>// e.g. &quot;this_method_name&quot; -&gt; &quot;thisMethodName&quot;
<br></span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/_(.?)/e&apos;</span><span class="keyword">,</span><span class="string">&quot;strtoupper(&apos;$1&apos;)&quot;</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);
<br>
<br></span><span class="comment">// camelcase (lower or upper) to underscored
<br>// e.g. &quot;thisMethodName&quot; -&gt; &quot;this_method_name&quot;
<br>// e.g. &quot;ThisMethodName&quot; -&gt; &quot;this_method_name&quot;
<br></span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/([^A-Z])([A-Z])/&apos;</span><span class="keyword">, </span><span class="string">&quot;$1_$2&quot;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>Of course these aren&apos;t 100% symmetric.&#xA0; For example...
<br>&#xA0; * this_is_a_string -&gt; ThisIsAString -&gt; this_is_astring
<br>&#xA0; * GetURLForString -&gt; get_urlfor_string -&gt; GetUrlforString</span>
</div>
  

#


<div class="phpcode"><span class="html">
Features:
<br>- multi byte compatible
<br>- handles multiple delimiters
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">ucwords_specific </span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$delimiters </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$encoding </span><span class="keyword">= </span><span class="default">NULL</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if (</span><span class="default">$encoding </span><span class="keyword">=== </span><span class="default">NULL</span><span class="keyword">) { </span><span class="default">$encoding </span><span class="keyword">= </span><span class="default">mb_internal_encoding</span><span class="keyword">();}
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$delimiters </span><span class="keyword">=&#xA0; </span><span class="default">str_split</span><span class="keyword">( </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$delimiters</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">$delimiters_pattern1 </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$delimiters_replace1 </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$delimiters_pattern2 </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$delimiters_replace2 </span><span class="keyword">= array();
<br>&#xA0; &#xA0; foreach (</span><span class="default">$delimiters </span><span class="keyword">as </span><span class="default">$delimiter</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$uniqid </span><span class="keyword">= </span><span class="default">uniqid</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$delimiters_pattern1</span><span class="keyword">[]&#xA0;&#xA0; = </span><span class="string">&apos;/&apos;</span><span class="keyword">. </span><span class="default">preg_quote</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">) .</span><span class="string">&apos;/&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$delimiters_replace1</span><span class="keyword">[]&#xA0;&#xA0; = </span><span class="default">$delimiter</span><span class="keyword">.</span><span class="default">$uniqid</span><span class="keyword">.</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$delimiters_pattern2</span><span class="keyword">[]&#xA0;&#xA0; = </span><span class="string">&apos;/&apos;</span><span class="keyword">. </span><span class="default">preg_quote</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">.</span><span class="default">$uniqid</span><span class="keyword">.</span><span class="string">&apos; &apos;</span><span class="keyword">) .</span><span class="string">&apos;/&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$delimiters_replace2</span><span class="keyword">[]&#xA0;&#xA0; = </span><span class="default">$delimiter</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// $return_string = mb_strtolower($string, $encoding);
<br>&#xA0; &#xA0; </span><span class="default">$return_string </span><span class="keyword">= </span><span class="default">$string</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$return_string </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="default">$delimiters_pattern1</span><span class="keyword">, </span><span class="default">$delimiters_replace1</span><span class="keyword">, </span><span class="default">$return_string</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$words </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">$return_string</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; foreach (</span><span class="default">$words </span><span class="keyword">as </span><span class="default">$index </span><span class="keyword">=&gt; </span><span class="default">$word</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">] = </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, </span><span class="default">$encoding</span><span class="keyword">), </span><span class="default">$encoding</span><span class="keyword">).</span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, </span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="default">$encoding</span><span class="keyword">), </span><span class="default">$encoding</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">$return_string </span><span class="keyword">= </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">$words</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$return_string </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="default">$delimiters_pattern2</span><span class="keyword">, </span><span class="default">$delimiters_replace2</span><span class="keyword">, </span><span class="default">$return_string</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$return_string</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Params:
<br>1. string: The string being converted
<br>2. delimiters: a string with all wanted delimiters written one after the other e.g. &quot;-&apos;&quot;
<br>3. encoding: Is the character encoding. If it is omitted, the internal character encoding value will be used.
<br>
<br>Example Usage:
<br><span class="default">&lt;?php
<br>mb_internal_encoding</span><span class="keyword">(</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">);
<br></span><span class="default">$string </span><span class="keyword">= </span><span class="string">&quot;JEAN-PAUL d&apos;artagnan &#x15F;&#x160;-&#xF2;&#xC0;-&#xE9;&#xCC; hello - world&quot;</span><span class="keyword">;
<br>echo </span><span class="default">ucwords_specific</span><span class="keyword">( </span><span class="default">mb_strtolower</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">), </span><span class="string">&quot;-&apos;&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Output:
<br>Jean-Paul D&apos;Artagnan &#x15E;&#x161;-&#xD2;&#xE0;-&#xC9;&#xEC; Hello - World</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ucwords.php)

**[⬆ to root](/)**