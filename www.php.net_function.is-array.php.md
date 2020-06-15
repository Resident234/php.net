# is_array




<div class="phpcode"><span class="html">
Please note that the &apos;cast to array&apos; check is horrendously out of date.<br><br>Running that code against PHP 5.6 results in this:<br><br>is_array&#xA0; :&#xA0; 0.93975400924683<br>cast, === :&#xA0; 1.2425191402435<br><br>So, please use &apos;is_array&apos;, not the horrible casting hack.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Or you could make use of the array_diff_key and array_key function:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) &amp;&amp; </span><span class="default">array_diff_key</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">,</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)));<br>}<br><br>function </span><span class="default">test</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) ? </span><span class="string">&quot;I&apos;m an assoc array.\n&quot; </span><span class="keyword">: </span><span class="string">&quot;I&apos;m not an assoc array.\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="comment">// an assoc array<br></span><span class="default">$a </span><span class="keyword">= array(</span><span class="string">&quot;a&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;aaa&quot;</span><span class="keyword">,</span><span class="string">&quot;b&quot;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">,</span><span class="string">&quot;c&quot;</span><span class="keyword">=&gt;</span><span class="default">true</span><span class="keyword">);<br></span><span class="default">test</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br><br></span><span class="comment">// an array<br></span><span class="default">$b </span><span class="keyword">= </span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br></span><span class="default">test</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">);<br><br></span><span class="comment">// an object<br></span><span class="default">$c </span><span class="keyword">= (object)</span><span class="default">$a</span><span class="keyword">;<br></span><span class="default">test</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">);<br><br></span><span class="comment">// other types<br></span><span class="default">test</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">a</span><span class="keyword">);<br></span><span class="default">test</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">b</span><span class="keyword">);<br></span><span class="default">test</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">c</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>The above code outputs:<br>I&apos;m an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Yet another simpler, faster is_assoc():<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) {<br>&#xA0; foreach (</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">$k </span><span class="keyword">!== </span><span class="default">$v</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; }<br>&#xA0; return </span><span class="default">false</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>In my tests it runs about twice as fast as Michael/Gabriel&apos;s array_reduce() method.<br><br>(Speaking of which: Gabriel&apos;s version doesn&apos;t work as written; it reports associative arrays as numeric if only the first key is non-numeric, or if the keys are numeric but ordered backwards.&#xA0; Michael solves this problem by comparing array_reduce() to count(), but that costs another function call; it also works to just compare to -1 instead of 0, and therefore return -1 as the ternary else from the callback).</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve found a faster way of determining an array. If you use is_array() millions of times, you will notice a *huge* difference. On my machine, this method takes about 1/4 the time of using is_array().
<br>
<br>Cast the value to an array, then check (using ===) if it is identical to the original.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if ( (array) </span><span class="default">$unknown </span><span class="keyword">!== </span><span class="default">$unknown </span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="string">&apos;$unknown is not an array&apos;</span><span class="keyword">;
<br>} else {
<br>&#xA0; &#xA0; echo </span><span class="string">&apos;$unknown is an array&apos;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>You can use this script to test the speed of both methods.
<br>
<br>&lt;pre&gt;
<br>What&apos;s faster for determining arrays?
<br>
<br><span class="default">&lt;?php
<br>
<br>$count </span><span class="keyword">= </span><span class="default">1000000</span><span class="keyword">;
<br>
<br></span><span class="default">$test </span><span class="keyword">= array(</span><span class="string">&apos;im&apos;</span><span class="keyword">, </span><span class="string">&apos;an&apos;</span><span class="keyword">, </span><span class="string">&apos;array&apos;</span><span class="keyword">);
<br></span><span class="default">$test2 </span><span class="keyword">= </span><span class="string">&apos;im not an array&apos;</span><span class="keyword">;
<br></span><span class="default">$test3 </span><span class="keyword">= (object) array(</span><span class="string">&apos;im&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;not&apos;</span><span class="keyword">, </span><span class="string">&apos;going&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;to be&apos;</span><span class="keyword">, </span><span class="string">&apos;an&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;array&apos;</span><span class="keyword">);
<br></span><span class="default">$test4 </span><span class="keyword">= </span><span class="default">42</span><span class="keyword">;
<br></span><span class="comment">// Set this now so the first for loop doesn&apos;t do the extra work.
<br></span><span class="default">$i </span><span class="keyword">= </span><span class="default">$start_time </span><span class="keyword">= </span><span class="default">$end_time </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br></span><span class="default">$start_time </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$count</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; if (!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$test</span><span class="keyword">) || </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$test2</span><span class="keyword">) || </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$test3</span><span class="keyword">) || </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$test4</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;error&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">$end_time </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>echo </span><span class="string">&apos;is_array&#xA0; :&#xA0; &apos;</span><span class="keyword">.(</span><span class="default">$end_time </span><span class="keyword">- </span><span class="default">$start_time</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$start_time </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$count</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; if (!(array) </span><span class="default">$test </span><span class="keyword">=== </span><span class="default">$test </span><span class="keyword">|| (array) </span><span class="default">$test2 </span><span class="keyword">=== </span><span class="default">$test2 </span><span class="keyword">|| (array) </span><span class="default">$test3 </span><span class="keyword">=== </span><span class="default">$test3 </span><span class="keyword">|| (array) </span><span class="default">$test4 </span><span class="keyword">=== </span><span class="default">$test4</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;error&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">$end_time </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>echo </span><span class="string">&apos;cast, === :&#xA0; &apos;</span><span class="keyword">.(</span><span class="default">$end_time </span><span class="keyword">- </span><span class="default">$start_time</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>
<br>echo </span><span class="string">&quot;\nTested </span><span class="default">$count</span><span class="string"> iterations.&quot;
<br>
<br></span><span class="default">?&gt;
<br></span>&lt;/pre&gt;
<br>
<br>Prints something like:
<br>
<br>What&apos;s faster for determining arrays?
<br>
<br>is_array&#xA0; :&#xA0; 7.9920151233673
<br>cast, === :&#xA0; 1.8978719711304
<br>
<br>Tested 1000000 iterations.</span>
</div>
  

#


<div class="phpcode"><span class="html">
hperrin&apos;s results have indeed changed in PHP 7. The opposite is now true, is_array is faster than comparison:<br><br>is_array&#xA0; :&#xA0; 0.52148389816284<br>cast, === :&#xA0; 0.84179711341858<br><br>Tested 1000000 iterations.</span>
</div>
  

#


<div class="phpcode"><span class="html">
yousef&apos;s example was wrong because is_vector returned true instead of false if the key was found<br>here is the fixed version (only 2 lines differ)<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_vector</span><span class="keyword">( &amp;</span><span class="default">$array </span><span class="keyword">) {<br>&#xA0;&#xA0; if ( !</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) || empty(</span><span class="default">$array</span><span class="keyword">) ) {<br>&#xA0; &#xA0; &#xA0; return -</span><span class="default">1</span><span class="keyword">;<br>&#xA0;&#xA0; }<br>&#xA0;&#xA0; </span><span class="default">$next </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0;&#xA0; foreach ( </span><span class="default">$array </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; if ( </span><span class="default">$k </span><span class="keyword">!== </span><span class="default">$next </span><span class="keyword">) return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$next</span><span class="keyword">++;<br>&#xA0;&#xA0; }<br>&#xA0;&#xA0; return </span><span class="default">true</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
alex frase&apos;s example is fast but elanthis at awesomeplay dot com&apos;s example is faster and Ilgar&apos;s modification of alex&apos;s code is faulty (the part &quot; || $_array[$k] !== $v&quot;). Also, Ilgar&apos;s suggestion of giving a false return value when the variable isnt an array is not suitable in my opinion and i think checking if the array is empty would also be a suitable check before the rest of the code runs.
<br>
<br>So here&apos;s the modified (is_vector) version
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">is_vector</span><span class="keyword">( &amp;</span><span class="default">$array </span><span class="keyword">) {
<br>&#xA0;&#xA0; if ( !</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) || empty(</span><span class="default">$array</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; return -</span><span class="default">1</span><span class="keyword">;
<br>&#xA0;&#xA0; }
<br>&#xA0;&#xA0; </span><span class="default">$next </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0;&#xA0; foreach ( </span><span class="default">$array </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; if ( </span><span class="default">$k </span><span class="keyword">!== </span><span class="default">$next </span><span class="keyword">) return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$next</span><span class="keyword">++;
<br>&#xA0;&#xA0; }
<br>&#xA0;&#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>and the modified (alex&apos;s is_assoc) version
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$_array</span><span class="keyword">) {
<br>&#xA0; &#xA0; if ( !</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$_array</span><span class="keyword">) || empty(</span><span class="default">$array</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return -</span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; foreach (</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$_array</span><span class="keyword">) as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$k </span><span class="keyword">!== </span><span class="default">$v</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I would change the order of the comparison, because if it is really an empty array, it is better to stop at that point before doing several &apos;cpu &amp; memory intensive&apos; function calls.<br><br>In the end on a ratio of 3 not empty arrays to 1 empty array computed for 1000000 iterations it needed 10% less time.<br>Or the other way round:<br>It needed approx 3% to 4% more time if the array is not empty, but was at least 4 times faster on empty arrays.<br><br>Additionally the memory consumption veritably lesser.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) {<br>&#xA0; &#xA0; return (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) &amp;&amp; (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)==</span><span class="default">0 </span><span class="keyword">|| </span><span class="default">0 </span><span class="keyword">!== </span><span class="default">count</span><span class="keyword">(</span><span class="default">array_diff_key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">))) )));<br>} <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
function is_associate_array($array)<br>{<br>&#xA0; &#xA0; return $array === array_values($array);<br>}<br><br>or you can add check is_array in functions</span>
</div>
  

#


<div class="phpcode"><span class="html">
The is_associative_array() and is_sequential_array() functions posted by &apos;rjg4013 at rit dot edu&apos; are not accurate.<br><br>The functions fail to recognize indexes that are not in sequence or in order.&#xA0; For example, array(0=&gt;&apos;a&apos;, 2=&gt;&apos;b&apos;, 1=&gt;&apos;c&apos;) and array(0=&gt;&apos;a&apos;, 3=&gt;&apos;b&apos;, 5=&gt;&apos;c&apos;) would be considered as sequential arrays. A true sequential array would be in consecutive order with no gaps in the indices.<br><br>The following solution utilizes the array_merge properties. If only one array is given and the array is numerically indexed, the keys get re-indexed in a continuous way.&#xA0; The result must match the array passed to it in order to truly be a numerically indexed (sequential) array.&#xA0; Otherwise it can be assumed to be an associative array (something unobtainable in languages such as C).<br><br>The following functions will work for PHP &gt;= 4.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">is_sequential_array</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return (</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) === </span><span class="default">$var </span><span class="keyword">&amp;&amp; </span><span class="default">is_numeric</span><span class="keyword">( </span><span class="default">implode</span><span class="keyword">( </span><span class="default">array_keys</span><span class="keyword">( </span><span class="default">$var </span><span class="keyword">) ) ) );<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; function </span><span class="default">is_assoc_array</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return (</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) !== </span><span class="default">$var </span><span class="keyword">|| !</span><span class="default">is_numeric</span><span class="keyword">( </span><span class="default">implode</span><span class="keyword">( </span><span class="default">array_keys</span><span class="keyword">( </span><span class="default">$var </span><span class="keyword">) ) ) );<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>If you are not concerned about the actual order of the indices, you can change the comparison to == and != respectively.</span>
</div>
  

#


<div class="phpcode"><span class="html">
And here is another variation for a function to test if an array is associative. Based on the idea by mot4h.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_associative</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)<br>{<br>&#xA0; if (!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) || empty(</span><span class="default">$array</span><span class="keyword">))<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br><br>&#xA0; </span><span class="default">$keys </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br>&#xA0; return </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$keys</span><span class="keyword">) !== </span><span class="default">$keys</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A slight modification of what&apos;s below:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) &amp;&amp; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) !== </span><span class="default">array_reduce</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">), </span><span class="string">&apos;is_assoc_callback&apos;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br>}<br><br>function </span><span class="default">is_assoc_callback</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">$a </span><span class="keyword">=== </span><span class="default">$b </span><span class="keyword">? </span><span class="default">$a </span><span class="keyword">+ </span><span class="default">1 </span><span class="keyword">: </span><span class="default">0</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using empty() in the previous example posted by Anonymous will result in a &quot;Fatal error: Can&apos;t use function return value in write context&quot;.&#xA0; I suggest using count() instead:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_assoc</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) {<br>&#xA0; &#xA0; return (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) &amp;&amp; </span><span class="default">0 </span><span class="keyword">!== </span><span class="default">count</span><span class="keyword">(</span><span class="default">array_diff_key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)))));<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-array.php)

**[To root](/README.md)**