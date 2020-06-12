# array_diff




<div class="phpcode"><span class="html">
Again, the function&apos;s description is misleading right now. I sought a function, which (mathematically) computes A - B, or, written differently, A \ B. Or, again in other words, suppose <br><br>A := {a1, ..., an} and B:= {a1, b1, ... , bm}<br><br>=&gt; array_diff(A,B) = {a2, ..., an}<br><br>array_diff(A,B) returns all elements from A, which are not elements of B (= A without B).<br><br>You should include this in the documentation more precisely, I think.</span>
</div>
  

#


<div class="phpcode"><span class="html">
array_diff provides a handy way of deleting array elements by their value, without having to unset it by key, through a lengthy foreach loop and then having to rekey the array.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">//pass value you wish to delete and the array to delete from<br></span><span class="keyword">function </span><span class="default">array_delete</span><span class="keyword">( </span><span class="default">$value</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">array_diff</span><span class="keyword">( </span><span class="default">$array</span><span class="keyword">, array(</span><span class="default">$value</span><span class="keyword">) );<br>&#xA0; &#xA0; return </span><span class="default">$array</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want a simple way to show values that are in either array, but not both, you can use this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">arrayDiff</span><span class="keyword">(</span><span class="default">$A</span><span class="keyword">, </span><span class="default">$B</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$intersect </span><span class="keyword">= </span><span class="default">array_intersect</span><span class="keyword">(</span><span class="default">$A</span><span class="keyword">, </span><span class="default">$B</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$A</span><span class="keyword">, </span><span class="default">$intersect</span><span class="keyword">), </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$B</span><span class="keyword">, </span><span class="default">$intersect</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;<br></span><br>If you want to account for keys, use array_diff_assoc() instead; and if you want to remove empty values, use array_filter().</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you just need to know if two arrays&apos; values are exactly the same (regardless of keys and order), then instead of using array_diff, this is a simple method:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">identical_values</span><span class="keyword">( </span><span class="default">$arrayA </span><span class="keyword">, </span><span class="default">$arrayB </span><span class="keyword">) {
<br>
<br>&#xA0; &#xA0; </span><span class="default">sort</span><span class="keyword">( </span><span class="default">$arrayA </span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">sort</span><span class="keyword">( </span><span class="default">$arrayB </span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$arrayA </span><span class="keyword">== </span><span class="default">$arrayB</span><span class="keyword">;
<br>}
<br>
<br></span><span class="comment">// Examples:
<br>
<br></span><span class="default">$array1 </span><span class="keyword">= array( </span><span class="string">&quot;red&quot; </span><span class="keyword">, </span><span class="string">&quot;green&quot; </span><span class="keyword">, </span><span class="string">&quot;blue&quot; </span><span class="keyword">);
<br></span><span class="default">$array2 </span><span class="keyword">= array( </span><span class="string">&quot;green&quot; </span><span class="keyword">, </span><span class="string">&quot;red&quot; </span><span class="keyword">, </span><span class="string">&quot;blue&quot; </span><span class="keyword">);
<br></span><span class="default">$array3 </span><span class="keyword">= array( </span><span class="string">&quot;red&quot; </span><span class="keyword">, </span><span class="string">&quot;green&quot; </span><span class="keyword">, </span><span class="string">&quot;blue&quot; </span><span class="keyword">, </span><span class="string">&quot;yellow&quot; </span><span class="keyword">);
<br></span><span class="default">$array4 </span><span class="keyword">= array( </span><span class="string">&quot;red&quot; </span><span class="keyword">, </span><span class="string">&quot;yellow&quot; </span><span class="keyword">, </span><span class="string">&quot;blue&quot; </span><span class="keyword">);
<br></span><span class="default">$array5 </span><span class="keyword">= array( </span><span class="string">&quot;x&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;red&quot; </span><span class="keyword">, </span><span class="string">&quot;y&quot; </span><span class="keyword">=&gt;&#xA0; </span><span class="string">&quot;green&quot; </span><span class="keyword">, </span><span class="string">&quot;z&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;blue&quot; </span><span class="keyword">);
<br>
<br></span><span class="default">identical_values</span><span class="keyword">( </span><span class="default">$array1 </span><span class="keyword">, </span><span class="default">$array2 </span><span class="keyword">);&#xA0; </span><span class="comment">// true
<br></span><span class="default">identical_values</span><span class="keyword">( </span><span class="default">$array1 </span><span class="keyword">, </span><span class="default">$array3 </span><span class="keyword">);&#xA0; </span><span class="comment">// false
<br></span><span class="default">identical_values</span><span class="keyword">( </span><span class="default">$array1 </span><span class="keyword">, </span><span class="default">$array4 </span><span class="keyword">);&#xA0; </span><span class="comment">// false
<br></span><span class="default">identical_values</span><span class="keyword">( </span><span class="default">$array1 </span><span class="keyword">, </span><span class="default">$array5 </span><span class="keyword">);&#xA0; </span><span class="comment">// true
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>The function returns true only if the two arrays contain the same number of values and each value in one array has an exact duplicate in the other array. Everything else will return false.
<br>
<br>my alternative method for evaluating if two arrays contain (all) identical values:
<br>
<br><span class="default">&lt;?php
<br>
<br>sort</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">); </span><span class="default">$sort</span><span class="keyword">(</span><span class="default">b</span><span class="keyword">); return </span><span class="default">$a </span><span class="keyword">== </span><span class="default">$b</span><span class="keyword">;
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>may be slightly faster (10-20%) than this array_diff method:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">return ( </span><span class="default">count</span><span class="keyword">( </span><span class="default">$a </span><span class="keyword">) == </span><span class="default">count</span><span class="keyword">( </span><span class="default">$b </span><span class="keyword">) &amp;&amp; !</span><span class="default">array_diff</span><span class="keyword">( </span><span class="default">$a </span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">) ? </span><span class="default">true </span><span class="keyword">: </span><span class="default">false </span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>but only when the two arrays contain the same number of values and then only in some cases. Otherwise the latter method will be radically faster due to the use of a count() test before the array_diff().
<br>
<br>Also, if the two arrays contain a different number of values, then which method is faster will depend on whether both arrays need to be sorted or not. Two times sort() is a bit slower than one time array_diff(), but if one of the arrays have already been sorted, then you only have to sort the other array and this will be almost twice as fast as array_diff().
<br>
<br>Basically: 2 x sort() is slower than 1 x array_diff() is slower than 1 x sort().</span>
</div>
  

#


<div class="phpcode"><span class="html">
I just came upon a really good use for array_diff(). When reading a dir(opendir;readdir), I _rarely_ want &quot;.&quot; or &quot;..&quot; to be in the array of files I&apos;m creating. Here&apos;s a simple way to remove them:
<br>
<br><span class="default">&lt;?php
<br> $someFiles </span><span class="keyword">= array();
<br> </span><span class="default">$dp </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="string">&quot;/some/dir&quot;</span><span class="keyword">);
<br> while(</span><span class="default">$someFiles</span><span class="keyword">[] = </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$dp</span><span class="keyword">));
<br> </span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$dp</span><span class="keyword">);
<br> 
<br> </span><span class="default">$removeDirs </span><span class="keyword">= array(</span><span class="string">&quot;.&quot;</span><span class="keyword">,</span><span class="string">&quot;..&quot;</span><span class="keyword">);
<br> </span><span class="default">$someFiles </span><span class="keyword">= </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$someFiles</span><span class="keyword">, </span><span class="default">$removeDirs</span><span class="keyword">);
<br> 
<br> foreach(</span><span class="default">$someFiles </span><span class="keyword">AS </span><span class="default">$thisFile</span><span class="keyword">) echo </span><span class="default">$thisFile</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>S</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hello guys,
<br>
<br>I&#xB4;ve been looking for a array_diff that works with recursive arrays, I&#xB4;ve tried the ottodenn at gmail dot com function but to my case it doesn&#xB4;t worked as expected, so I made my own. I&#xB4;ve haven&#xB4;t tested this extensively, but I&#xB4;ll explain my scenario, and this works great at that case :D
<br>
<br>We got 2 arrays like these:
<br>
<br><span class="default">&lt;?php
<br>$aArray1</span><span class="keyword">[</span><span class="string">&apos;marcie&apos;</span><span class="keyword">] = array(</span><span class="string">&apos;banana&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;orange&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;pasta&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$aArray1</span><span class="keyword">[</span><span class="string">&apos;kenji&apos;</span><span class="keyword">] = array(</span><span class="string">&apos;apple&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;pie&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;pasta&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);
<br>
<br></span><span class="default">$aArray2</span><span class="keyword">[</span><span class="string">&apos;marcie&apos;</span><span class="keyword">] = array(</span><span class="string">&apos;banana&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;orange&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>As array_diff, this function returns all the items that is in aArray1 and IS NOT at aArray2, so the result we should expect is:
<br>
<br><span class="default">&lt;?php
<br>$aDiff</span><span class="keyword">[</span><span class="string">&apos;marcie&apos;</span><span class="keyword">] = array(</span><span class="string">&apos;pasta&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$aDiff</span><span class="keyword">[</span><span class="string">&apos;kenji&apos;</span><span class="keyword">] = array(</span><span class="string">&apos;apple&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;pie&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;pasta&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Ok, now some comments about this function:
<br> - Different from the PHP array_diff, this function DON&#xB4;T uses the === operator, but the ==, so 0 is equal to &apos;0&apos; or false, but this can be changed with no impacts.
<br> - This function checks the keys of the arrays, array_diff only compares the values.
<br>
<br>I realy hopes that this could help some1 as I&#xB4;ve been helped a lot with some users experiences. (Just please double check if it would work for your case, as I sad I just tested to a scenario like the one I exposed)
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">arrayRecursiveDiff</span><span class="keyword">(</span><span class="default">$aArray1</span><span class="keyword">, </span><span class="default">$aArray2</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$aReturn </span><span class="keyword">= array();
<br>&#xA0;&#xA0; 
<br>&#xA0; &#xA0; foreach (</span><span class="default">$aArray1 </span><span class="keyword">as </span><span class="default">$mKey </span><span class="keyword">=&gt; </span><span class="default">$mValue</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$mKey</span><span class="keyword">, </span><span class="default">$aArray2</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$mValue</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$aRecursiveDiff </span><span class="keyword">= </span><span class="default">arrayRecursiveDiff</span><span class="keyword">(</span><span class="default">$mValue</span><span class="keyword">, </span><span class="default">$aArray2</span><span class="keyword">[</span><span class="default">$mKey</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$aRecursiveDiff</span><span class="keyword">)) { </span><span class="default">$aReturn</span><span class="keyword">[</span><span class="default">$mKey</span><span class="keyword">] = </span><span class="default">$aRecursiveDiff</span><span class="keyword">; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$mValue </span><span class="keyword">!= </span><span class="default">$aArray2</span><span class="keyword">[</span><span class="default">$mKey</span><span class="keyword">]) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$aReturn</span><span class="keyword">[</span><span class="default">$mKey</span><span class="keyword">] = </span><span class="default">$mValue</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$aReturn</span><span class="keyword">[</span><span class="default">$mKey</span><span class="keyword">] = </span><span class="default">$mValue</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0;&#xA0; 
<br>&#xA0; &#xA0; return </span><span class="default">$aReturn</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is more fast implementation of array_diff, but with some limitations. If you need compare two arrays of integers or strings you can use such function:<br><br>&#xA0; &#xA0; public static function arrayDiffEmulation($arrayFrom, $arrayAgainst)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $arrayAgainst = array_flip($arrayAgainst);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; foreach ($arrayFrom as $key =&gt; $value) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset($arrayAgainst[$value])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($arrayFrom[$key]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return $arrayFrom;<br>&#xA0; &#xA0; }<br><br>It is ~10x faster than array_diff<br><br>php &gt; $t = microtime(true);$a = range(0,25000); $b = range(15000,500000); $c = array_diff($a, $b);echo microtime(true) - $t;<br>4.4335179328918<br>php &gt; $t = microtime(true);$a = range(0,25000); $b = range(15000,500000); $c = arrayDiffEmulation($a, $b);echo microtime(true) - $t;<br>0.37219095230103</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-diff.php)

**[⬆ to root](/)**