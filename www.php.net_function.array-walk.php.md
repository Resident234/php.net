# array_walk




<div class="phpcode"><span class="html">
It&apos;s worth nothing that array_walk can not be used to change keys in the array.<br>The function may be defined as (&amp;$value, $key) but not (&amp;$value, &amp;$key).<br>Even though PHP does not complain/warn, it does not modify the key.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Calling an array Walk inside a class <br><br>If the class is static:<br>array_walk($array, array(&apos;self&apos;, &apos;walkFunction&apos;));<br>or<br>array_walk($array, array(&apos;className&apos;, &apos;walkFunction&apos;));<br><br>Otherwise:<br>array_walk($array, array($this, &apos;walkFunction&apos;));</span>
</div>
  

#


<div class="phpcode"><span class="html">
I noticed that :<br><br>PHP ignored arguments type when using array_walk() even if there was<br> <br>declare(strict_types=1) . <br><br>See this code as an example ...<br><br><span class="default">&lt;?php<br></span><span class="keyword">declare(</span><span class="default">strict_types</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">);<br><br></span><span class="default">$fruits </span><span class="keyword">= array(</span><span class="string">&quot;butter&quot; </span><span class="keyword">=&gt; </span><span class="default">5.3</span><span class="keyword">, </span><span class="string">&quot;meat&quot; </span><span class="keyword">=&gt; </span><span class="default">7</span><span class="keyword">, </span><span class="string">&quot;banana&quot; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">);<br><br>function </span><span class="default">test_print</span><span class="keyword">(</span><span class="default">int $item2</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$key</span><span class="string">: </span><span class="default">$item2</span><span class="string">&lt;br /&gt;\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">array_walk</span><span class="keyword">(</span><span class="default">$fruits</span><span class="keyword">, </span><span class="string">&apos;test_print&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>The output is :<br><br>butter: 5<br>meat: 7<br>banana: 3<br><br>whilst the expecting output is :<br><br>Fatal error: Uncaught TypeError: Argument 1 passed to test_print() must be of the type integer<br><br>because &quot;butter&quot; =&gt; 5.3 is float<br><br>I asked someone about it and they said &quot;this was caused by the fact that callbacks called from internal code will always use weak type&quot;. But I tried to do some tests and this behavior is not an issue when using call_user_func().</span>
</div>
  

#


<div class="phpcode"><span class="html">
// We can make that with this simple FOREACH loop : <br><br>$fruits = array(&quot;d&quot; =&gt; &quot;lemon&quot;, &quot;a&quot; =&gt; &quot;orange&quot;, &quot;b&quot; =&gt; &quot;banana&quot;, &quot;c&quot; =&gt; &quot;apple&quot;);<br><br>foreach($fruits as $cls =&gt; $vls)<br>{<br>&#xA0; $fruits[$cls] = &quot;fruit: &quot;.$vls;<br>}<br><br>Results: <br><br>Array<br>(<br>&#xA0; &#xA0; [d] =&gt; fruit: lemon<br>&#xA0; &#xA0; [a] =&gt; fruit: orange<br>&#xA0; &#xA0; [b] =&gt; fruit: banana<br>&#xA0; &#xA0; [c] =&gt; fruit: apple<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that using array_walk with intval is inappropriate.<br>There are many examples on internet that suggest to use following code to safely escape $_POST arrays of integers:<br><span class="default">&lt;?php<br>array_walk</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;something&apos;</span><span class="keyword">],</span><span class="string">&apos;intval&apos;</span><span class="keyword">); </span><span class="comment">// does nothing in PHP 5.3.3<br></span><span class="default">?&gt;<br></span>It works in _some_ older PHP versions (5.2), but is against specifications. Since intval() does not modify it&apos;s arguments, but returns modified result, the code above has no effect on the array and will leave security hole in your website.<br><br>You can use following instead:<br><span class="default">&lt;?php<br>$_POST</span><span class="keyword">[</span><span class="string">&apos;something&apos;</span><span class="keyword">] = </span><span class="default">array_map</span><span class="keyword">(</span><span class="default">intval</span><span class="keyword">,</span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;something&apos;</span><span class="keyword">]);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It can be very useful to pass the third (optional) parameter by reference while modifying it permanently in callback function. This will cause passing modified parameter to next iteration of array_walk(). The exaple below enumerates items in the array:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">enumerate</span><span class="keyword">( &amp;</span><span class="default">$item1</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">, &amp;</span><span class="default">$startNum </span><span class="keyword">) {
<br>&#xA0;&#xA0; </span><span class="default">$item1 </span><span class="keyword">= </span><span class="default">$startNum</span><span class="keyword">++ .</span><span class="string">&quot; </span><span class="default">$item1</span><span class="string">&quot;</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">$num </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>
<br></span><span class="default">$fruits </span><span class="keyword">= array( </span><span class="string">&quot;lemon&quot;</span><span class="keyword">, </span><span class="string">&quot;orange&quot;</span><span class="keyword">, </span><span class="string">&quot;banana&quot;</span><span class="keyword">, </span><span class="string">&quot;apple&quot;</span><span class="keyword">);
<br></span><span class="default">array_walk</span><span class="keyword">(</span><span class="default">$fruits</span><span class="keyword">, </span><span class="string">&apos;enumerate&apos;</span><span class="keyword">, </span><span class="default">$num </span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$fruits </span><span class="keyword">);
<br>
<br>echo </span><span class="string">&apos;$num is: &apos;</span><span class="keyword">. </span><span class="default">$num </span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>This outputs:
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; 1 lemon
<br>&#xA0; &#xA0; [1] =&gt; 2 orange
<br>&#xA0; &#xA0; [2] =&gt; 3 banana
<br>&#xA0; &#xA0; [3] =&gt; 4 apple
<br>)
<br>$num is: 1
<br>
<br>Notice at the last line of output that outside of array_walk() the $num parameter has initial value of 1. This is because array_walk() does not take the third parameter by reference.. so what if we pass the reference as the optional parameter..
<br>
<br><span class="default">&lt;?php
<br>$num </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>
<br></span><span class="default">$fruits </span><span class="keyword">= array( </span><span class="string">&quot;lemon&quot;</span><span class="keyword">, </span><span class="string">&quot;orange&quot;</span><span class="keyword">, </span><span class="string">&quot;banana&quot;</span><span class="keyword">, </span><span class="string">&quot;apple&quot;</span><span class="keyword">);
<br></span><span class="default">array_walk</span><span class="keyword">(</span><span class="default">$fruits</span><span class="keyword">, </span><span class="string">&apos;enumerate&apos;</span><span class="keyword">, &amp;</span><span class="default">$num </span><span class="keyword">); </span><span class="comment">// reference here
<br>
<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$fruits </span><span class="keyword">);
<br>
<br>echo </span><span class="string">&apos;$num is: &apos;</span><span class="keyword">. </span><span class="default">$num </span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;we&apos;ve got &quot;</span><span class="keyword">. (</span><span class="default">$num </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">) .</span><span class="string">&quot; fruits in the basket!&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span> 
<br>This outputs:
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; 1 lemon
<br>&#xA0; &#xA0; [1] =&gt; 2 orange
<br>&#xA0; &#xA0; [2] =&gt; 3 banana
<br>&#xA0; &#xA0; [3] =&gt; 4 apple
<br>)
<br>$num is: 5
<br>we&apos;ve got 4 fruits in the basket!
<br>
<br>Now $num has changed so we are able to count the items (without calling count() unnecessarily).
<br>
<br>As a conclusion, using references with array_walk() can be powerful toy but this should be done carefully since modifying third parameter outside the array_walk() is not always what we want.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to unset elements from the callback function, maybe what you really need is array_filter.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Don&apos;t forget about the array_map() function, it may be easier to use!<br><br>Here&apos;s how to lower-case all elements in an array:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $arr </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;strtolower&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-walk.php)

**[⬆ to root](/)**