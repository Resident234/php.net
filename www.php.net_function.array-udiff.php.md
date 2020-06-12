# array_udiff




<div class="phpcode"><span class="html">
I think the example given here using classes is convoluting things too much to demonstrate what this function does.<br><br>array_udiff() will walk through array_values($a) and array_values($b) and compare each value by using the passed in callback function.<br><br>To put it another way, array_udiff() compares $a[0] to $b[0], $b[1], $b[2], and $b[3] using the provided callback function.&#xA0; If the callback returns zero for any of the comparisons then $a[0] will not be in the returned array from array_udiff().&#xA0; It then compares $a[1] to $b[0], $b[1], $b[2], and $b[3].&#xA0; Then, finally, $a[2] to $b[0], $b[1], $b[2], and $b[3].<br><br>For example, compare_ids($a[0], $b[0]) === -5 while compare_ids($a[1], $b[1]) === 0.&#xA0; Therefore, $a[1] is not returned from array_udiff() since it is present in $b.<br><br>&lt;?<br>$a = array(<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 10,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;John&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;red&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 20,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Elise&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;blue&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 30,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Mark&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;red&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>);<br><br>$b = array(<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 15,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Nancy&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;black&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 20,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Elise&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;blue&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 30,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Mark&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;red&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 40,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;John&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;color&apos; =&gt; &apos;orange&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>);<br><br>function compare_ids($a, $b)<br>{<br>&#xA0; &#xA0; return ($a[&apos;id&apos;] - $b[&apos;id&apos;]);<br>}<br>function compare_names($a, $b)<br>{<br>&#xA0; &#xA0; return strcmp($a[&apos;name&apos;], $b[&apos;name&apos;]);<br>}<br><br>$ret = array_udiff($a, $b, &apos;compare_ids&apos;);<br>var_dump($ret);<br><br>$ret = array_udiff($b, $a, &apos;compare_ids&apos;);<br>var_dump($ret);<br><br>$ret = array_udiff($a, $b, &apos;compare_names&apos;);<br>var_dump($ret);<br>?&gt;<br><br>Which returns the following.<br><br>In the first return we see that $b has no entry in it with an id of 10.<br>&lt;?<br>array(1) {<br>&#xA0; [0]=&gt;<br>&#xA0; array(3) {<br>&#xA0; &#xA0; [&quot;id&quot;]=&gt;<br>&#xA0; &#xA0; int(10)<br>&#xA0; &#xA0; [&quot;name&quot;]=&gt;<br>&#xA0; &#xA0; string(4) &quot;John&quot;<br>&#xA0; &#xA0; [&quot;color&quot;]=&gt;<br>&#xA0; &#xA0; string(3) &quot;red&quot;<br>&#xA0; }<br>}<br>?&gt;<br><br>In the second return we see that $a has no entry in it with an id of 15 or 40.<br>&lt;?<br>array(2) {<br>&#xA0; [0]=&gt;<br>&#xA0; array(3) {<br>&#xA0; &#xA0; [&quot;id&quot;]=&gt;<br>&#xA0; &#xA0; int(15)<br>&#xA0; &#xA0; [&quot;name&quot;]=&gt;<br>&#xA0; &#xA0; string(5) &quot;Nancy&quot;<br>&#xA0; &#xA0; [&quot;color&quot;]=&gt;<br>&#xA0; &#xA0; string(5) &quot;black&quot;<br>&#xA0; }<br>&#xA0; [3]=&gt;<br>&#xA0; array(3) {<br>&#xA0; &#xA0; [&quot;id&quot;]=&gt;<br>&#xA0; &#xA0; int(40)<br>&#xA0; &#xA0; [&quot;name&quot;]=&gt;<br>&#xA0; &#xA0; string(4) &quot;John&quot;<br>&#xA0; &#xA0; [&quot;color&quot;]=&gt;<br>&#xA0; &#xA0; string(6) &quot;orange&quot;<br>&#xA0; }<br>}<br>?&gt;<br><br>In third return we see that all names in $a are in $b (even though the entry in $b whose name is &apos;John&apos; is different, the anonymous function is only comparing names).<br>&lt;?<br>array(0) {<br>}<br>?&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that the compare function is used also internally, to order the arrays and choose which element compare against in the next round.<br><br>If your compare function is not really comparing (ie. returns 0 if elements are equals, 1 otherwise), you will receive an unexpected result.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is not stated, by this function also diffs array1 to itself, removing any duplicate values...</span>
</div>
  

#


<div class="phpcode"><span class="html">
Quick example for using array_udiff to do a multi-dimensional diff
<br>
<br>Returns values of $arr1 that are not in $arr2
<br>
<br><span class="default">&lt;?php
<br>$arr1 </span><span class="keyword">= array( array(</span><span class="string">&apos;Bob&apos;</span><span class="keyword">, </span><span class="default">42</span><span class="keyword">), array(</span><span class="string">&apos;Phil&apos;</span><span class="keyword">, </span><span class="default">37</span><span class="keyword">), array(</span><span class="string">&apos;Frank&apos;</span><span class="keyword">, </span><span class="default">39</span><span class="keyword">) );
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="default">$arr2 </span><span class="keyword">= array( array(</span><span class="string">&apos;Phil&apos;</span><span class="keyword">, </span><span class="default">37</span><span class="keyword">), array(</span><span class="string">&apos;Mark&apos;</span><span class="keyword">, </span><span class="default">45</span><span class="keyword">) );
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="default">$arr3 </span><span class="keyword">= </span><span class="default">array_udiff</span><span class="keyword">(</span><span class="default">$arr1</span><span class="keyword">, </span><span class="default">$arr2</span><span class="keyword">, </span><span class="default">create_function</span><span class="keyword">(
<br>&#xA0; &#xA0; </span><span class="string">&apos;$a,$b&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;return strcmp( implode(&quot;&quot;, $a), implode(&quot;&quot;, $b) ); &apos;</span><span class="keyword">)
<br>&#xA0; &#xA0; );
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$arr3</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Output:
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Bob
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 42
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br> 
<br>&#xA0; &#xA0; [2] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Frank
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 39
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br> 
<br>)
<br>1
<br>
<br>Hope this helps someone</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-udiff.php)

**[⬆ to root](/)**