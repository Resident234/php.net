# str_word_count




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="comment">/***<br> * This simple utf-8 word count function (it only counts) <br> * is a bit faster then the one with preg_match_all<br> * about 10x slower then the built-in str_word_count<br> * <br> * If you need the hyphen or other code points as word-characters<br> * just put them into the [brackets] like [^\p{L}\p{N}\&apos;\-]<br> * If the pattern contains utf-8, utf8_encode() the pattern,<br> * as it is expected to be valid utf-8 (using the u modifier).<br> **/<br><br>// Jonny 5&apos;s simple word splitter<br></span><span class="keyword">function </span><span class="default">str_word_count_utf8</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) {<br>&#xA0; return </span><span class="default">count</span><span class="keyword">(</span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;~[^\p{L}\p{N}\&apos;]+~u&apos;</span><span class="keyword">,</span><span class="default">$str</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
We can also specify a range of values for charlist.
<br>
<br><span class="default">&lt;?php
<br>$str </span><span class="keyword">= </span><span class="string">&quot;Hello fri3nd, you&apos;re
<br>&#xA0; &#xA0; &#xA0;&#xA0; looking&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; good today! 
<br>&#xA0; &#xA0; &#xA0;&#xA0; look1234ing&quot;</span><span class="keyword">;
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">str_word_count</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;0..3&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>will give the result as 
<br>
<br>Array ( [0] =&gt; Hello [1] =&gt; fri3nd [2] =&gt; you&apos;re [3] =&gt; looking [4] =&gt; good [5] =&gt; today [6] =&gt; look123 [7] =&gt; ing )</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-word-count.php)

**[⬆ to root](/)**