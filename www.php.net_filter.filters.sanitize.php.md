# Sanitize filters




<div class="phpcode"><span class="html">
FILTER_SANITIZE_STRING doesn&apos;t behavior the same as strip_tags function.&#xA0; &#xA0; strip_tags allows less than symbol inferred from context, FILTER_SANITIZE_STRING strips regardless.<br><span class="default">&lt;?php<br>$smaller </span><span class="keyword">= </span><span class="string">&quot;not a tag &lt; 5&quot;</span><span class="keyword">;<br>echo </span><span class="default">strip_tags</span><span class="keyword">(</span><span class="default">$smaller</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">// -&gt; not a tag &lt; 5<br></span><span class="keyword">echo </span><span class="default">filter_var </span><span class="keyword">( </span><span class="default">$smaller</span><span class="keyword">, </span><span class="default">FILTER_SANITIZE_STRING</span><span class="keyword">); </span><span class="comment">// -&gt; not a tag<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Remember to trim() the $_POST before your filters are applied:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// We trim the $_POST data before any spaces get encoded to &quot;%20&quot;<br><br>// Trim array values using this function &quot;trim_value&quot;<br></span><span class="keyword">function </span><span class="default">trim_value</span><span class="keyword">(&amp;</span><span class="default">$value</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">// this removes whitespace and related characters from the beginning and end of the string<br></span><span class="keyword">}<br></span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">, </span><span class="string">&apos;trim_value&apos;</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">// the data in $_POST is trimmed<br><br></span><span class="default">$postfilter </span><span class="keyword">=&#xA0; &#xA0; </span><span class="comment">// set up the filters to be used with the trimmed post array<br>&#xA0; &#xA0; </span><span class="keyword">array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;user_tasks&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt;&#xA0; &#xA0; array(</span><span class="string">&apos;filter&apos; </span><span class="keyword">=&gt; </span><span class="default">FILTER_SANITIZE_STRING</span><span class="keyword">, </span><span class="string">&apos;flags&apos; </span><span class="keyword">=&gt; !</span><span class="default">FILTER_FLAG_STRIP_LOW</span><span class="keyword">),&#xA0; &#xA0; </span><span class="comment">// removes tags. formatting code is encoded -- add nl2br() when displaying<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;username&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt;&#xA0; &#xA0; array(</span><span class="string">&apos;filter&apos; </span><span class="keyword">=&gt; </span><span class="default">FILTER_SANITIZE_ENCODED</span><span class="keyword">, </span><span class="string">&apos;flags&apos; </span><span class="keyword">=&gt; </span><span class="default">FILTER_FLAG_STRIP_LOW</span><span class="keyword">),&#xA0; &#xA0; </span><span class="comment">// we are using this in the url<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;mod_title&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt;&#xA0; &#xA0; array(</span><span class="string">&apos;filter&apos; </span><span class="keyword">=&gt; </span><span class="default">FILTER_SANITIZE_ENCODED</span><span class="keyword">, </span><span class="string">&apos;flags&apos; </span><span class="keyword">=&gt; </span><span class="default">FILTER_FLAG_STRIP_LOW</span><span class="keyword">),&#xA0; &#xA0; </span><span class="comment">// we are using this in the url<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br><br></span><span class="default">$revised_post_array </span><span class="keyword">= </span><span class="default">filter_var_array</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">, </span><span class="default">$postfilter</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">// must be referenced via a variable which is now an array that takes the place of $_POST[]<br></span><span class="keyword">echo (</span><span class="default">nl2br</span><span class="keyword">(</span><span class="default">$revised_post_array</span><span class="keyword">[</span><span class="string">&apos;user_tasks&apos;</span><span class="keyword">]));&#xA0; &#xA0; </span><span class="comment">//-- use nl2br() upon output like so, for the [&apos;user_tasks&apos;] array value so that the newlines are formatted, since this is our HTML &lt;textarea&gt; field and we want to maintain newlines<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To include multiple flags, simply separate the flags with vertical pipe symbols.<br><br>For example, if you want to use filter_var() to sanitize $string with FILTER_SANITIZE_STRING and pass in FILTER_FLAG_STRIP_HIGH and FILTER_FLAG_STRIP_LOW, just call it like this:<br><br>$string = filter_var($string, FILTER_SANITIZE_STRING, FILTER_FLAG_STRIP_HIGH | FILTER_FLAG_STRIP_LOW);<br><br>The same goes for passing a flags field in an options array in the case of using callbacks.<br><br>$var = filter_var($string, FILTER_SANITIZE_SPECIAL_CHARS,<br>array(&apos;flags&apos; =&gt; FILTER_FLAG_STRIP_LOW | FILTER_FLAG_ENCODE_HIGH));<br><br>Thanks to the Brain Goo blog at popmartian.com/tipsntricks/for this info.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s not entirely clear what the LOW and HIGH ranges are. LOW is characters below 32, HIGH is those above 127, i.e. outside the ASCII range.<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="string">&quot;\tcaf&#xE9;\n&quot;</span><span class="keyword">;<br></span><span class="comment">//This will remove the tab and the line break<br></span><span class="keyword">echo </span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">FILTER_SANITIZE_STRING</span><span class="keyword">, </span><span class="default">FILTER_FLAG_STRIP_LOW</span><span class="keyword">);<br></span><span class="comment">//This will remove the &#xE9;.<br></span><span class="keyword">echo </span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">FILTER_SANITIZE_STRING</span><span class="keyword">, </span><span class="default">FILTER_FLAG_STRIP_HIGH</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just to clarify, since this may be unknown for a lot of people:
<br>
<br>ASCII characters above 127 are known as &quot;Extended&quot; and they represent characters such as greek letters and accented letters in latin alphabets, used in languages such as pt_BR.
<br>
<br>A good ASCII quick reference (aside from the already mentioned Wikipedia article) can be found at: <a href="http://www.asciicodes.com/" rel="nofollow" target="_blank">http://www.asciicodes.com/</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.sanitize.php)

**[⬆ to root](/)**