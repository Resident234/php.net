# array_sum




<div class="phpcode"><span class="html">
If you want to find the AVERAGE of the values in your array, use the sum and count functions together.&#xA0; For example, let&apos;s say your array is $foo and you want the average...
<br>
<br><span class="default">&lt;?php
<br>$average_of_foo </span><span class="keyword">= </span><span class="default">array_sum</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">) / </span><span class="default">count</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to check if there are for example only strings in an array, you can use a combination of array_sum and array_map like this:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">only_strings_in_array</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="default">array_sum</span><span class="keyword">(</span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;is_string&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">)) == </span><span class="default">count</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br>}<br><br></span><span class="default">$arr1 </span><span class="keyword">= array(</span><span class="string">&apos;one&apos;</span><span class="keyword">, </span><span class="string">&apos;two&apos;</span><span class="keyword">, </span><span class="string">&apos;three&apos;</span><span class="keyword">);<br></span><span class="default">$arr2 </span><span class="keyword">= array(</span><span class="string">&apos;foo&apos;</span><span class="keyword">, </span><span class="string">&apos;bar&apos;</span><span class="keyword">, array());<br></span><span class="default">$arr3 </span><span class="keyword">= array(</span><span class="string">&apos;foo&apos;</span><span class="keyword">, array(), </span><span class="string">&apos;bar&apos;</span><span class="keyword">);<br></span><span class="default">$arr4 </span><span class="keyword">= array(array(), </span><span class="string">&apos;foo&apos;</span><span class="keyword">, </span><span class="string">&apos;bar&apos;</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(<br>&#xA0; &#xA0; </span><span class="default">only_strings_in_array</span><span class="keyword">(</span><span class="default">$arr1</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="default">only_strings_in_array</span><span class="keyword">(</span><span class="default">$arr2</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="default">only_strings_in_array</span><span class="keyword">(</span><span class="default">$arr3</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="default">only_strings_in_array</span><span class="keyword">(</span><span class="default">$arr4</span><span class="keyword">)<br>);<br></span><span class="default">?&gt;<br></span><br>This will give you the following result:<br>bool(true)<br>bool(false)<br>bool(false)<br>bool(false)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-sum.php)

**[⬆ to root](/)**