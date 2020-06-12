# ucfirst




<div class="phpcode"><span class="html">
Simple multi-bytes ucfirst():<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">my_mb_ucfirst</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$fc </span><span class="keyword">= </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">));<br>&#xA0; &#xA0; return </span><span class="default">$fc</span><span class="keyword">.</span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A proper Turkish solution;<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">ucfirst_turkish</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&quot;//u&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">mb_convert_case</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;i&quot;</span><span class="keyword">, </span><span class="string">&quot;&#x130;&quot;</span><span class="keyword">, </span><span class="default">$tmp</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]), </span><span class="default">MB_CASE_TITLE</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">).<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tmp</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">];<br>}<br><br></span><span class="default">$str </span><span class="keyword">= </span><span class="string">&quot;iyilik g&#xFC;zelL&#x130;K&quot;</span><span class="keyword">;<br>echo </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) .</span><span class="string">&quot;\n&quot;</span><span class="keyword">;&#xA0;&#xA0; </span><span class="comment">// Iyilik g&#xFC;zelL&#x130;K<br></span><span class="keyword">echo </span><span class="default">ucfirst_turkish</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">); </span><span class="comment">// &#x130;yilik g&#xFC;zelL&#x130;K<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I believe that mb_ucfirst will be soon added in PHP, but for now this could be useful<br><span class="default">&lt;?php<br><br></span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;mb_ucfirst&apos;</span><span class="keyword">) &amp;&amp; </span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;mb_substr&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; function </span><span class="default">mb_ucfirst</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">)) . </span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$string</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;<br></span><br>it also check is mb support enabled or not</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a function to capitalize segments of a name, and put the rest into lower case. You can pass the characters you want to use as delimiters.<br><br>i.e. <span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">nameize</span><span class="keyword">(</span><span class="string">&quot;john o&apos;grady-smith&quot;</span><span class="keyword">); </span><span class="default">?&gt;<br></span><br>returns John O&apos;Grady-Smith<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">nameize</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">,</span><span class="default">$a_char </span><span class="keyword">= array(</span><span class="string">&quot;&apos;&quot;</span><span class="keyword">,</span><span class="string">&quot;-&quot;</span><span class="keyword">,</span><span class="string">&quot; &quot;</span><span class="keyword">)){&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">//$str contains the complete raw name string<br>&#xA0; &#xA0; //$a_char is an array containing the characters we use as separators for capitalization. If you don&apos;t pass anything, there are three in there as default.<br>&#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">);<br>&#xA0; &#xA0; foreach (</span><span class="default">$a_char </span><span class="keyword">as </span><span class="default">$temp</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pos </span><span class="keyword">= </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">,</span><span class="default">$temp</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$pos</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//we are in the loop because we found one of the special characters in the array, so lets split it up into chunks and capitalize each one.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$mend </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$a_split </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$temp</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$a_split </span><span class="keyword">as </span><span class="default">$temp2</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//capitalize each portion of the string which was separated at a special character<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$mend </span><span class="keyword">.= </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">$temp2</span><span class="keyword">).</span><span class="default">$temp</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$mend</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,-</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is what I use for converting strings to sentence case:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">sentence_case</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$sentences </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;/([.?!]+)/&apos;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY</span><span class="keyword">|</span><span class="default">PREG_SPLIT_DELIM_CAPTURE</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$new_string </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; foreach (</span><span class="default">$sentences </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$sentence</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$new_string </span><span class="keyword">.= (</span><span class="default">$key </span><span class="keyword">&amp; </span><span class="default">1</span><span class="keyword">) == </span><span class="default">0</span><span class="keyword">?
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$sentence</span><span class="keyword">))) :
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$sentence</span><span class="keyword">.</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$new_string</span><span class="keyword">);
<br>}
<br>
<br>print </span><span class="default">sentence_case</span><span class="keyword">(</span><span class="string">&apos;HMM. WOW! WHAT?&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// Outputs: &quot;Hmm. Wow! What?&quot;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ucfirst.php)

**[⬆ to root](/)**