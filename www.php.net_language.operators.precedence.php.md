# Operator Precedence




<div class="phpcode"><span class="html">
Watch out for the difference of priority between &apos;and vs &amp;&amp;&apos; or &apos;|| vs or&apos;:<br><span class="default">&lt;?php<br>$bool </span><span class="keyword">= </span><span class="default">true </span><span class="keyword">&amp;&amp; </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$bool</span><span class="keyword">); </span><span class="comment">// false, that&apos;s expected<br><br></span><span class="default">$bool </span><span class="keyword">= </span><span class="default">true </span><span class="keyword">and </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$bool</span><span class="keyword">); </span><span class="comment">// true, ouch!<br></span><span class="default">?&gt;<br></span>Because &apos;and/or&apos; have lower priority than &apos;=&apos; but &apos;||/&amp;&amp;&apos; have higher.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware the unusual order of bit-wise operators and comparison operators, this has often lead to bugs in my experience. For instance:<br><br><span class="default">&lt;?php </span><span class="keyword">if ( </span><span class="default">$flags </span><span class="keyword">&amp; </span><span class="default">MASK&#xA0; </span><span class="keyword">== </span><span class="default">1</span><span class="keyword">) </span><span class="default">do_something</span><span class="keyword">(); </span><span class="default">?&gt;<br></span><br>will not do what you might expect from other languages. Use<br><br><span class="default">&lt;?php </span><span class="keyword">if ((</span><span class="default">$flags </span><span class="keyword">&amp; </span><span class="default">MASK</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">) </span><span class="default">do_something</span><span class="keyword">(); </span><span class="default">?&gt;<br></span><br>in PHP instead.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;ve come here looking for a full list of PHP operators, take note that the table here is *not* complete. There are some additional operators (or operator-ish punctuation tokens) that are not included here, such as &quot;-&gt;&quot;, &quot;::&quot;, and &quot;...&quot;.<br><br>For a really comprehensive list, take a look at the &quot;List of Parser Tokens&quot; page: <a href="http://php.net/manual/en/tokens.php" rel="nofollow" target="_blank">http://php.net/manual/en/tokens.php</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.precedence.php)

**[To root](/)**