# array_splice




<div class="phpcode"><span class="html">
array_splice, split an array into 2 arrays. The returned arrays is the 2nd argument actually and the used array e.g $input here contains the 1st argument of array, e.g
<br>
<br><span class="default">&lt;?php
<br>$input </span><span class="keyword">= array(</span><span class="string">&quot;red&quot;</span><span class="keyword">, </span><span class="string">&quot;green&quot;</span><span class="keyword">, </span><span class="string">&quot;blue&quot;</span><span class="keyword">, </span><span class="string">&quot;yellow&quot;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">)); </span><span class="comment">// Array ( [0] =&gt; yellow )&#xA0; 
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">); </span><span class="comment">//Array ( [0] =&gt; red [1] =&gt; green [2] =&gt; blue )
<br></span><span class="default">?&gt;
<br></span>
<br>if you want to replace any array value do simple like that,
<br>
<br>first search the array index you want to replace
<br>
<br><span class="default">&lt;?php $index </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&apos;green&apos;</span><span class="keyword">, </span><span class="default">$input</span><span class="keyword">);</span><span class="comment">// index = 1 </span><span class="default">?&gt;
<br></span>
<br>and then use it as according to the definition
<br>
<br><span class="default">&lt;?php
<br>array_splice</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$index</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, array(</span><span class="string">&apos;mygreeen&apos;</span><span class="keyword">)); </span><span class="comment">//Array ( [0] =&gt; red [1] =&gt; mygreeen [2] =&gt; blue [3] =&gt; yellow ) 
<br></span><span class="default">?&gt;
<br></span>
<br>so here green is replaced by mygreen.
<br>
<br>here 1 in array_splice above represent the number of items to be replaced. so here start at index &apos;1&apos; and replaced only one item which is &apos;green&apos;</span>
</div>
  

#


<div class="phpcode"><span class="html">
You cannot insert with array_splice an array with your own key. array_splice will always insert it with the key &quot;0&quot;.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// [DATA]
<br></span><span class="default">$test_array </span><span class="keyword">= array (
<br>&#xA0; </span><span class="default">row1 </span><span class="keyword">=&gt; array (</span><span class="default">col1 </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar!&apos;</span><span class="keyword">, </span><span class="default">col2 </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar!&apos;</span><span class="keyword">),
<br>&#xA0; </span><span class="default">row2 </span><span class="keyword">=&gt; array (</span><span class="default">col1 </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar!&apos;</span><span class="keyword">, </span><span class="default">col2 </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar!&apos;</span><span class="keyword">),
<br>&#xA0; </span><span class="default">row3 </span><span class="keyword">=&gt; array (</span><span class="default">col1 </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar!&apos;</span><span class="keyword">, </span><span class="default">col2 </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar!&apos;</span><span class="keyword">)
<br>);
<br>
<br></span><span class="comment">// [ACTION]
<br></span><span class="default">array_splice </span><span class="keyword">(</span><span class="default">$test_array</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, array (</span><span class="string">&apos;rowX&apos; </span><span class="keyword">=&gt; array (</span><span class="string">&apos;colX&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar2&apos;</span><span class="keyword">)));
<br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">; </span><span class="default">print_r </span><span class="keyword">(</span><span class="default">$test_array</span><span class="keyword">); echo </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>[RESULT]
<br>
<br>Array (
<br>&#xA0; &#xA0; [row1] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col1] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col2] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [row2] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col1] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col2] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [0] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [colX] =&gt; foobar2
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [row3] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col1] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col2] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>)
<br>
<br>But you can use the following function:
<br>
<br>function array_insert (&amp;$array, $position, $insert_array) {
<br>&#xA0; $first_array = array_splice ($array, 0, $position);
<br>&#xA0; $array = array_merge ($first_array, $insert_array, $array);
<br>}
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// [ACTION]
<br>
<br></span><span class="default">array_insert </span><span class="keyword">(</span><span class="default">$test_array</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, array (</span><span class="string">&apos;rowX&apos; </span><span class="keyword">=&gt; array (</span><span class="string">&apos;colX&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;foobar2&apos;</span><span class="keyword">)));
<br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">; </span><span class="default">print_r </span><span class="keyword">(</span><span class="default">$test_array</span><span class="keyword">); echo </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>[RESULT]
<br>
<br>Array (
<br>&#xA0; &#xA0; [row1] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col1] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col2] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [row2] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col1] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col2] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [rowX] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [colX] =&gt; foobar2
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [row3] =&gt; Array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col1] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [col2] =&gt; foobar!
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>)
<br>
<br>[NOTE]
<br>
<br>The position &quot;0&quot; will insert the array in the first position (like array_shift). If you try a position higher than the langth of the array, you add it to the array like the function array_push.</span>
</div>
  

#


<div class="phpcode"><span class="html">
just useful functions to move an element using array_splice.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// info at danielecentamore dot com<br><br>// $input&#xA0; (Array) - the array containing the element<br>// $index (int) - the index of the element you need to move<br><br></span><span class="keyword">function </span><span class="default">moveUp</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">,</span><span class="default">$index</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$new_array </span><span class="keyword">= </span><span class="default">$input</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0;&#xA0; if((</span><span class="default">count</span><span class="keyword">(</span><span class="default">$new_array</span><span class="keyword">)&gt;</span><span class="default">$index</span><span class="keyword">) &amp;&amp; (</span><span class="default">$index</span><span class="keyword">&gt;</span><span class="default">0</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$new_array</span><span class="keyword">, </span><span class="default">$index</span><span class="keyword">-</span><span class="default">1</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$input</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$new_array</span><span class="keyword">, </span><span class="default">$index</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } <br><br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">$new_array</span><span class="keyword">;<br>}<br><br>function </span><span class="default">moveDown</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">,</span><span class="default">$index</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$new_array </span><span class="keyword">= </span><span class="default">$input</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0;&#xA0; if(</span><span class="default">count</span><span class="keyword">(</span><span class="default">$new_array</span><span class="keyword">)&gt;</span><span class="default">$index</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$new_array</span><span class="keyword">, </span><span class="default">$index</span><span class="keyword">+</span><span class="default">2</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$input</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$new_array</span><span class="keyword">, </span><span class="default">$index</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } <br>&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">$new_array</span><span class="keyword">;<br> }&#xA0; <br><br></span><span class="default">$input </span><span class="keyword">= array(</span><span class="string">&quot;red&quot;</span><span class="keyword">, </span><span class="string">&quot;green&quot;</span><span class="keyword">, </span><span class="string">&quot;blue&quot;</span><span class="keyword">, </span><span class="string">&quot;yellow&quot;</span><span class="keyword">);<br><br></span><span class="default">$newinput </span><span class="keyword">= </span><span class="default">moveUp</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);<br></span><span class="comment">// $newinput is array(&quot;red&quot;, &quot;blue&quot;, &quot;green&quot;, &quot;yellow&quot;)<br><br></span><span class="default">$input </span><span class="keyword">= </span><span class="default">moveDown</span><span class="keyword">(</span><span class="default">$newinput</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br></span><span class="comment">// $input is array(&quot;red&quot;, &quot;green&quot;, &quot;blue&quot;, &quot;yellow&quot;)<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When trying to splice an associative array into another, array_splice is missing two key ingredients:<br>&#xA0; - a string key for identifying the offset<br>&#xA0; - the ability to preserve keys in the replacement array<br><br>This is primarily useful when you want to replace an item in an array with another item, but want to maintain the ordering of the array without rebuilding the array one entry at a time.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_splice_assoc</span><span class="keyword">(&amp;</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$offset</span><span class="keyword">, </span><span class="default">$length</span><span class="keyword">, </span><span class="default">$replacement</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$replacement </span><span class="keyword">= (array) </span><span class="default">$replacement</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$key_indices </span><span class="keyword">= </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$input</span><span class="keyword">[</span><span class="default">$offset</span><span class="keyword">]) &amp;&amp; </span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$offset </span><span class="keyword">= </span><span class="default">$key_indices</span><span class="keyword">[</span><span class="default">$offset</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$input</span><span class="keyword">[</span><span class="default">$length</span><span class="keyword">]) &amp;&amp; </span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$length</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$length </span><span class="keyword">= </span><span class="default">$key_indices</span><span class="keyword">[</span><span class="default">$length</span><span class="keyword">] - </span><span class="default">$offset</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$input </span><span class="keyword">= </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$offset</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; + </span><span class="default">$replacement<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">+ </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$offset </span><span class="keyword">+ </span><span class="default">$length</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">);<br>}<br><br></span><span class="default">$fruit </span><span class="keyword">= array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;orange&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;orange&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;lemon&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;yellow&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;lime&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;green&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;grape&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;purple&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;cherry&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;red&apos;</span><span class="keyword">,<br>);<br><br></span><span class="comment">// Replace lemon and lime with apple<br></span><span class="default">array_splice_assoc</span><span class="keyword">(</span><span class="default">$fruit</span><span class="keyword">, </span><span class="string">&apos;lemon&apos;</span><span class="keyword">, </span><span class="string">&apos;grape&apos;</span><span class="keyword">, array(</span><span class="string">&apos;apple&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;red&apos;</span><span class="keyword">));<br><br></span><span class="comment">// Replace cherry with strawberry<br></span><span class="default">array_splice_assoc</span><span class="keyword">(</span><span class="default">$fruit</span><span class="keyword">, </span><span class="string">&apos;cherry&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, array(</span><span class="string">&apos;strawberry&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;red&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Note: I have not tested this with negative offsets and lengths.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-splice.php)

**[To root](/README.md)**