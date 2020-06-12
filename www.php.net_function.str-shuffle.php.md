# str_shuffle




<div class="phpcode"><span class="html">
Aoccdrnig to rseearch at an Elingsh uinervtisy, it deosn&apos;t mttaer in waht oredr the ltteers in a wrod are, the olny iprmoatnt tihng is that the frist and lsat ltteer is at the rghit pclae. The rset can be a toatl mses and you can sitll raed it wouthit a porbelm. Tihs is bcuseae we do not raed ervey lteter by it slef but the wrod as a wlohe.<br><br>Hree&apos;s a cdoe taht slerbmcas txet in tihs way:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">scramble_word</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">) &lt; </span><span class="default">2</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$word</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$word</span><span class="keyword">{</span><span class="default">0</span><span class="keyword">} . </span><span class="default">str_shuffle</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">)) . </span><span class="default">$word</span><span class="keyword">{</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">};<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; echo </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/(\w+)/e&apos;</span><span class="keyword">, </span><span class="string">&apos;scramble_word(&quot;\1&quot;)&apos;</span><span class="keyword">, </span><span class="string">&apos;A quick brown fox jumped over the lazy dog.&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>It may be ufseul if you wnat to cetare an aessblicce CTCPAHA.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function is affected by srand():<br><br><span class="default">&lt;?php<br>srand</span><span class="keyword">(</span><span class="default">12345</span><span class="keyword">);<br>echo </span><span class="default">str_shuffle</span><span class="keyword">(</span><span class="string">&apos;Randomize me&apos;</span><span class="keyword">) . </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">; </span><span class="comment">// &quot;demmiezr aon&quot;<br></span><span class="keyword">echo </span><span class="default">str_shuffle</span><span class="keyword">(</span><span class="string">&apos;Randomize me&apos;</span><span class="keyword">) . </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">; </span><span class="comment">// &quot;izadmeo rmen&quot;<br><br></span><span class="default">srand</span><span class="keyword">(</span><span class="default">12345</span><span class="keyword">);<br>echo </span><span class="default">str_shuffle</span><span class="keyword">(</span><span class="string">&apos;Randomize me&apos;</span><span class="keyword">) . </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">; </span><span class="comment">// &quot;demmiezr aon&quot; again<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A proper unicode string shuffle;<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">str_shuffle_unicode</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&quot;//u&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">shuffle</span><span class="keyword">(</span><span class="default">$tmp</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">join</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">, </span><span class="default">$tmp</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>$str = &quot;&#x15E;eker y&#xE2;rim&quot;; // My sweet love<br><br>echo str_shuffle($str); // i&#xFFFD;eymr&#x162;ekr &#xFFFD;<br><br>echo str_shuffle_unicode($str); // &#x15E;r mreyeik&#xE2;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-shuffle.php)

**[⬆ to root](/)**