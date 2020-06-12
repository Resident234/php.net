# count




<div class="phpcode"><span class="html">
[Editor&apos;s note: array at from dot pl had pointed out that count() is a cheap operation; however, there&apos;s still the function call overhead.]
<br>
<br>If you want to run through large arrays don&apos;t use count() function in the loops , its a over head in performance,&#xA0; copy the count() value into a variable and use that value in loops for a better performance.
<br>
<br>Eg:
<br>
<br>// Bad approach
<br>
<br>for($i=0;$i&lt;count($some_arr);$i++)
<br>{
<br>&#xA0; &#xA0; // calculations
<br>}
<br>
<br>// Good approach
<br>
<br>$arr_length = count($some_arr);
<br>for($i=0;$i&lt;$arr_length;$i++)
<br>{
<br>&#xA0; &#xA0; // calculations
<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
My function returns the number of elements in array for multidimensional arrays subject to depth of array. (Almost COUNT_RECURSIVE, but you can point on which depth you want to plunge).
<br>
<br><span class="default">&lt;?php
<br>&#xA0; </span><span class="keyword">function </span><span class="default">getArrCount </span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; if (!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">) || !</span><span class="default">$depth</span><span class="keyword">) return </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$res</span><span class="keyword">=</span><span class="default">count</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$in_ar</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$res</span><span class="keyword">+=</span><span class="default">getArrCount</span><span class="keyword">(</span><span class="default">$in_ar</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">-</span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">$res</span><span class="keyword">;
<br>&#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.count.php)

**[⬆ to root](/)**