# array_filter




<div class="phpcode"><span class="html">
If you want a quick way to remove NULL, FALSE and Empty Strings (&quot;&quot;), but leave values of 0 (zero), you can use the standard php function strlen as the callback function:<br>eg:<br><span class="default">&lt;?php<br><br></span><span class="comment">// removes all NULL, FALSE and Empty Strings but leaves 0 (zero) values<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">array_filter</span><span class="keyword">( </span><span class="default">$array</span><span class="keyword">, </span><span class="string">&apos;strlen&apos; </span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Because array_filter() preserves keys, you should consider the resulting array to be an associative array even if the original array had integer keys for there may be holes in your sequence of keys. This means that, for example, json_encode() will convert your result array into an object instead of an array. Call array_values() on the result array to guarantee json_encode() gives you an array.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In case you are interested (like me) in filtering out elements with certain key-names, array_filter won&apos;t help you. Instead you can use the following:<br><br><span class="default">&lt;?php<br>$arr </span><span class="keyword">= array( </span><span class="string">&apos;element1&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;element2&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;element3&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;element4&apos; </span><span class="keyword">=&gt; </span><span class="default">4 </span><span class="keyword">);<br></span><span class="default">$filterOutKeys </span><span class="keyword">= array( </span><span class="string">&apos;element1&apos;</span><span class="keyword">, </span><span class="string">&apos;element4&apos; </span><span class="keyword">);<br><br></span><span class="default">$filteredArr </span><span class="keyword">= </span><span class="default">array_diff_key</span><span class="keyword">( </span><span class="default">$arr</span><span class="keyword">, </span><span class="default">array_flip</span><span class="keyword">( </span><span class="default">$filterOutKeys </span><span class="keyword">) )<br></span><span class="default">?&gt;<br></span><br>Result will be something like this:<br>[&apos;element2&apos;] =&gt; 2<br>[&apos;element3&apos;] =&gt; 3</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is how you could easily delete a specific value from an array with array_filter:
<br>
<br><span class="default">&lt;?php
<br>$array </span><span class="keyword">= array (</span><span class="default">1</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">);
<br></span><span class="default">$my_value </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">;
<br></span><span class="default">$filtered_array </span><span class="keyword">= </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, function (</span><span class="default">$element</span><span class="keyword">) use (</span><span class="default">$my_value</span><span class="keyword">) { return (</span><span class="default">$element </span><span class="keyword">!= </span><span class="default">$my_value</span><span class="keyword">); } );
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$filtered_array</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>output:
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; 1
<br>&#xA0; &#xA0; [3] =&gt; 5
<br>&#xA0; &#xA0; [4] =&gt; 6
<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
array_filter remove also FALSE and 0. To remove only NULL&apos;s use:<br><br>$af = [1, 0, 2, null, 3, 6, 7];<br><br>function is_not_null($val){<br>&#xA0; &#xA0; return !is_null($val);<br>}<br>$af = array_filter($af, &apos;is_not_null&apos;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a function that will filter a multi-demensional array. This filter will return only those items that match the $value given
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="comment">/*
<br>&#xA0; &#xA0;&#xA0; * filtering an array
<br>&#xA0; &#xA0;&#xA0; */
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">filter_by_value </span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$index</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">){ 
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) &amp;&amp; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)&gt;</span><span class="default">0</span><span class="keyword">)&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; { 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) as </span><span class="default">$key</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$temp</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">][</span><span class="default">$index</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$temp</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] == </span><span class="default">$value</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newarray</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">$newarray</span><span class="keyword">; 
<br>&#xA0; &#xA0; } 
<br></span><span class="default">?&gt;
<br></span>
<br>Example:
<br>
<br><span class="default">&lt;?php
<br>$results </span><span class="keyword">= array(
<br>&#xA0;&#xA0; </span><span class="default">0 </span><span class="keyword">=&gt; array(</span><span class="string">&apos;key1&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;1&apos;</span><span class="keyword">, </span><span class="string">&apos;key2&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;key3&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">),
<br>&#xA0;&#xA0; </span><span class="default">1 </span><span class="keyword">=&gt; array(</span><span class="string">&apos;key1&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;12&apos;</span><span class="keyword">, </span><span class="string">&apos;key2&apos; </span><span class="keyword">=&gt; </span><span class="default">22</span><span class="keyword">, </span><span class="string">&apos;key3&apos; </span><span class="keyword">=&gt; </span><span class="default">32</span><span class="keyword">) 
<br>);
<br>
<br></span><span class="default">$nResults </span><span class="keyword">= </span><span class="default">filter_by_value</span><span class="keyword">(</span><span class="default">$results</span><span class="keyword">, </span><span class="string">&apos;key2&apos;</span><span class="keyword">, </span><span class="string">&apos;2&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Output :
<br>
<br>array(
<br>&#xA0; &#xA0; 0 =&gt; array(&apos;key1&apos; =&gt; &apos;1&apos;, &apos;key2&apos; =&gt; 2, &apos;key3&apos; =&gt; 3)
<br>);</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to use array_filter with a class method as the callback, you can use a psuedo type callback like this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Test<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doFilter</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;callbackMethodName&apos;</span><span class="keyword">));<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; protected function </span><span class="default">callbackMethodName</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$element </span><span class="keyword">% </span><span class="default">2 </span><span class="keyword">=== </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$example </span><span class="keyword">= new </span><span class="default">Test</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$example</span><span class="keyword">-&gt;</span><span class="default">doFilter</span><span class="keyword">(</span><span class="default">range</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">, </span><span class="default">10</span><span class="keyword">)));<br></span><span class="default">?&gt;<br></span><br>Will return even numbers.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some of PHP&apos;s array functions play a prominent role in so called functional programming languages, where they show up under a slightly different name:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; array_filter</span><span class="keyword">() -&gt; </span><span class="default">filter</span><span class="keyword">(), 
<br>&#xA0; </span><span class="default">array_map</span><span class="keyword">() -&gt; </span><span class="default">map</span><span class="keyword">(), 
<br>&#xA0; </span><span class="default">array_reduce</span><span class="keyword">() -&gt; </span><span class="default">foldl</span><span class="keyword">() (</span><span class="string">&quot;fold left&quot;</span><span class="keyword">)
<br></span><span class="default">?&gt;
<br></span>
<br>Functional programming is a paradigm which centers around the side-effect free evaluation of functions. A program execution is a call of a function, which in turn might be defined by many other functions. One idea is to use functions to create special purpose functions from other functions.
<br>
<br>The array functions mentioned above allow you compose new functions on arrays. 
<br>
<br>E.g. array_sum = array_map(&quot;sum&quot;, $arr).
<br>
<br>This leads to a style of programming that looks much like algebra, e.g. the Bird/Meertens formalism.
<br>
<br>E.g. a mathematician might state
<br>
<br>&#xA0; map(f o g) = map(f) o map(g)
<br>
<br>the so called &quot;loop fusion&quot; law.
<br>
<br>Many functions on arrays can be created by the use of the foldr() function (which works like foldl, but eating up array elements from the right).
<br>
<br>I can&apos;t get into detail here, I just wanted to provide a hint about where this stuff also shows up and the theory behind it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function filters an array and remove all null values recursively.
<br>
<br><span class="default">&lt;?php
<br>&#xA0; </span><span class="keyword">function </span><span class="default">array_filter_recursive</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">)
<br>&#xA0; {
<br>&#xA0; &#xA0; foreach (</span><span class="default">$input </span><span class="keyword">as &amp;</span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">array_filter_recursive</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; return </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">);
<br>&#xA0; }
<br></span><span class="default">?&gt;
<br></span>
<br>Or with callback parameter (not tested) :
<br>
<br><span class="default">&lt;?php
<br>&#xA0; </span><span class="keyword">function </span><span class="default">array_filter_recursive</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$callback </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">)
<br>&#xA0; {
<br>&#xA0; &#xA0; foreach (</span><span class="default">$input </span><span class="keyword">as &amp;</span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">array_filter_recursive</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$callback</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; return </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$callback</span><span class="keyword">);
<br>&#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can access the current key of array by passing a reference to array into callback function and call key() and next() method in the callback function:<br><span class="default">&lt;?php<br>$data </span><span class="keyword">= array(</span><span class="string">&apos;first&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;second&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;third&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">);<br></span><span class="default">$data </span><span class="keyword">= </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">, function (</span><span class="default">$item</span><span class="keyword">) use (&amp;</span><span class="default">$data</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Filtering key &quot;</span><span class="keyword">, </span><span class="default">key</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">), </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">, </span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">next</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>});<br></span><span class="default">?&gt;<br></span><br>However be careful with array internal pointer or use reset() method before calling array_filter().</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-filter.php)

**[To root](/README.md)**