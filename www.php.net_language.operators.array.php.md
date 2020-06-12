# Array Operators




<div class="phpcode"><span class="html">
The union operator did not behave as I thought it would on first glance. It implements a union (of sorts) based on the keys of the array, not on the values.<br><br>For instance:<br><span class="default">&lt;?php<br>$a </span><span class="keyword">= array(</span><span class="string">&apos;one&apos;</span><span class="keyword">,</span><span class="string">&apos;two&apos;</span><span class="keyword">);<br></span><span class="default">$b</span><span class="keyword">=array(</span><span class="string">&apos;three&apos;</span><span class="keyword">,</span><span class="string">&apos;four&apos;</span><span class="keyword">,</span><span class="string">&apos;five&apos;</span><span class="keyword">);<br><br></span><span class="comment">//not a union of arrays&apos; values<br></span><span class="keyword">echo </span><span class="string">&apos;$a + $b : &apos;</span><span class="keyword">;<br></span><span class="default">print_r </span><span class="keyword">(</span><span class="default">$a </span><span class="keyword">+ </span><span class="default">$b</span><span class="keyword">);<br><br></span><span class="comment">//a union of arrays&apos; values<br></span><span class="keyword">echo </span><span class="string">&quot;array_unique(array_merge(</span><span class="default">$a</span><span class="string">,</span><span class="default">$b</span><span class="string">)):&quot;</span><span class="keyword">;<br></span><span class="comment">// cribbed from <a href="http://oreilly.com/catalog/progphp/chapter/ch05.html" rel="nofollow" target="_blank">http://oreilly.com/catalog/progphp/chapter/ch05.html</a><br></span><span class="default">print_r </span><span class="keyword">(</span><span class="default">array_unique</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">,</span><span class="default">$b</span><span class="keyword">)));<br></span><span class="default">?&gt;<br></span><br>//output<br><br>$a + $b : Array<br>(<br>&#xA0; &#xA0; [0] =&gt; one<br>&#xA0; &#xA0; [1] =&gt; two<br>&#xA0; &#xA0; [2] =&gt; five<br>)<br>array_unique(array_merge(Array,Array)):Array<br>(<br>&#xA0; &#xA0; [0] =&gt; one<br>&#xA0; &#xA0; [1] =&gt; two<br>&#xA0; &#xA0; [2] =&gt; three<br>&#xA0; &#xA0; [3] =&gt; four<br>&#xA0; &#xA0; [4] =&gt; five<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
The example may get u into thinking that the identical operator returns true because the key of apple is a string but that is not the case, cause if a string array key is the standart representation of a integer it&apos;s gets a numeral key automaticly. <br><br>The identical operator just requires that the keys are in the same order in both arrays:<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= array (</span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&quot;banana&quot;</span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= array (</span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&quot;banana&quot;</span><span class="keyword">, </span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&quot;apple&quot;</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a </span><span class="keyword">=== </span><span class="default">$b</span><span class="keyword">); </span><span class="comment">// prints bool(false) as well<br><br></span><span class="default">$b </span><span class="keyword">= array (</span><span class="string">&quot;0&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="string">&quot;1&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;banana&quot;</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a </span><span class="keyword">=== </span><span class="default">$b</span><span class="keyword">); </span><span class="comment">// prints bool(true)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be mentioned that the array union operator functions almost identically to array_replace with the exception that precedence of arguments is reversed.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that + will not renumber numeric array keys.&#xA0; If you have two numeric arrays, and their indices overlap, + will use the first array&apos;s values for each numeric key, adding the 2nd array&apos;s values only where the first doesn&apos;t already have a value for that index.&#xA0; Example:<br><br>$a = array(&apos;red&apos;, &apos;orange&apos;);<br>$b = array(&apos;yellow&apos;, &apos;green&apos;, &apos;blue&apos;);<br>$both = $a + $b;<br>var_dump($both);<br><br>Produces the output:<br><br>array(3) { [0]=&gt;&#xA0; string(3) &quot;red&quot; [1]=&gt;&#xA0; string(6) &quot;orange&quot; [2]=&gt;&#xA0; string(4) &quot;blue&quot; }<br><br>To get a 5-element array, use array_merge.<br><br>&#xA0; &#xA0; Dan</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.array.php)

**[To root](/)**