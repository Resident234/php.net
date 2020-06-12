# array_keys




<div class="phpcode"><span class="html">
It&apos;s worth noting that if you have keys that are long integer, such as &apos;329462291595&apos;, they will be considered as such on a 64bits system, but will be of type string on a 32 bits system.<br><br>for example:<br><span class="default">&lt;?php <br><br>$importantKeys </span><span class="keyword">= array(</span><span class="string">&apos;329462291595&apos; </span><span class="keyword">=&gt;</span><span class="default">null</span><span class="keyword">, </span><span class="string">&apos;ZZ291595&apos; </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">);<br><br>foreach(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$importantKeys</span><span class="keyword">) as </span><span class="default">$key</span><span class="keyword">){<br>&#xA0; &#xA0; echo </span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>will return on a 64 bits system:<br><span class="default">&lt;?php <br>&#xA0; &#xA0; integer<br>&#xA0; &#xA0; string<br>?&gt;<br></span><br>but on a 32 bits system:<br><span class="default">&lt;?php <br>&#xA0; &#xA0; string<br>&#xA0; &#xA0; string<br>?&gt;<br></span><br>I hope it will save someone the huge headache I had :)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s how to get the first key, the last key, the first value or the last value of a (hash) array without explicitly copying nor altering the original array:<br><br><span class="default">&lt;?php<br>&#xA0; $array </span><span class="keyword">= array(</span><span class="string">&apos;first&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;111&apos;</span><span class="keyword">, </span><span class="string">&apos;second&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;222&apos;</span><span class="keyword">, </span><span class="string">&apos;third&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;333&apos;</span><span class="keyword">);<br><br>&#xA0; </span><span class="comment">// get the first key: returns &apos;first&apos;<br>&#xA0; </span><span class="keyword">print </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">));<br><br>&#xA0; </span><span class="comment">// get the last key: returns &apos;third&apos;<br>&#xA0; </span><span class="keyword">print </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">));<br><br>&#xA0; </span><span class="comment">// get the first value: returns &apos;111&apos;<br>&#xA0; </span><span class="keyword">print </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">));<br><br>&#xA0; </span><span class="comment">// get the last value: returns &apos;333&apos;<br>&#xA0; </span><span class="keyword">print </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Since 5.4 STRICT standards dictate that you cannot wrap array_keys in a function like array_shift that attempts to reference the array.&#xA0; <br><br>Invalid:<br>echo array_shift( array_keys( array(&apos;a&apos; =&gt; &apos;apple&apos;) ) );<br><br>Valid:<br>$keys = array_keys( array(&apos;a&apos; =&gt; &apos;apple&apos;) );<br>echo array_shift( $keys );<br><br>But Wait! Since PHP (currently) allows you to break a reference by wrapping a variable in parentheses, you can currently use:<br><br>echo array_shift( ( array_keys( array(&apos;a&apos; =&gt; &apos;apple&apos;) ) ) );<br><br>However I would expect in time the PHP team will modify the rules of parentheses.</span>
</div>
  

#


<div class="phpcode"><span class="html">
There&apos;s a lot of multidimensional array_keys function out there, but each of them only merges all the keys in one flat array.<br><br>Here&apos;s a way to find all the keys from a multidimensional&#xA0; array while keeping the array structure. An optional MAXIMUM DEPTH parameter can be set for testing purpose in case of very large arrays.<br><br>NOTE: If the sub element isn&apos;t an array, it will be ignore.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_keys_recursive</span><span class="keyword">(</span><span class="default">$myArray</span><span class="keyword">, </span><span class="default">$MAXDEPTH </span><span class="keyword">= </span><span class="default">INF</span><span class="keyword">, </span><span class="default">$depth </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">, </span><span class="default">$arrayKeys </span><span class="keyword">= array()){<br>&#xA0; &#xA0; &#xA0;&#xA0; if(</span><span class="default">$depth </span><span class="keyword">&lt; </span><span class="default">$MAXDEPTH</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$depth</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$myArray</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$keys </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$myArray</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$arrayKeys</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">array_keys_recursive</span><span class="keyword">(</span><span class="default">$myArray</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">], </span><span class="default">$MAXDEPTH</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$arrayKeys</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>EXAMPLE:<br>input:<br>array(<br>&#xA0; &#xA0; &apos;Player&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; &apos;4&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;state&apos; =&gt; &apos;active&apos;,<br>&#xA0; &#xA0; ),<br>&#xA0; &#xA0; &apos;LevelSimulation&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; &apos;1&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;simulation_id&apos; =&gt; &apos;1&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;level_id&apos; =&gt; &apos;1&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;Level&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; &apos;1&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;city_id&apos; =&gt; &apos;8&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;City&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; &apos;8&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;class&apos; =&gt; &apos;home&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; ),<br>&#xA0; &#xA0; &apos;User&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; &apos;48&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;gender&apos; =&gt; &apos;M&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;group&apos; =&gt; &apos;user&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;username&apos; =&gt; &apos;Hello&apos;<br>&#xA0; &#xA0; )<br>)<br><br>output:<br>array(<br>&#xA0; &#xA0; &apos;Player&apos; =&gt; array(),<br>&#xA0; &#xA0; &apos;LevelSimulation&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;Level&apos; =&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;City&apos; =&gt; array()<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; ),<br>&#xA0; &#xA0; &apos;User&apos; =&gt; array()<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sorry for my english...
<br>
<br>I wrote a function to get keys of arrays recursivelly...
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">recursive_keys</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$search_value </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">){
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output </span><span class="keyword">= (</span><span class="default">$search_value </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$search_value</span><span class="keyword">) : </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">)) ;
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$input </span><span class="keyword">as </span><span class="default">$sub</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$sub</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output </span><span class="keyword">= (</span><span class="default">$search_value </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$output</span><span class="keyword">, </span><span class="default">recursive_keys</span><span class="keyword">(</span><span class="default">$sub</span><span class="keyword">, </span><span class="default">$search_value</span><span class="keyword">)) : </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$output</span><span class="keyword">, </span><span class="default">recursive_keys</span><span class="keyword">(</span><span class="default">$sub</span><span class="keyword">))) ;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$output </span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;
<br></span>
<br>I hope it will be usefull
<br>
<br>Regards</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-keys.php)

**[⬆ to root](/)**