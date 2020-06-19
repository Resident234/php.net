# in_array




<div class="phpcode"><span class="html">
Loose checking returns some crazy, counter-intuitive results when used with certain arrays. It is completely correct behaviour, due to PHP&apos;s leniency on variable types, but in &quot;real-life&quot; is almost useless.<br><br>The solution is to use the strict checking option.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// Example array<br><br></span><span class="default">$array </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;egg&apos; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;cheese&apos; </span><span class="keyword">=&gt; </span><span class="default">false</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;hair&apos; </span><span class="keyword">=&gt; </span><span class="default">765</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;goblins&apos; </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;ogres&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;no ogres allowed in this array&apos;<br></span><span class="keyword">);<br><br></span><span class="comment">// Loose checking -- return values are in comments<br><br>// First three make sense, last four do not<br><br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">false</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">765</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">763</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="string">&apos;egg&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="string">&apos;hhh&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(array(), </span><span class="default">$array</span><span class="keyword">); </span><span class="comment">// true<br><br>// Strict checking<br><br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">false</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">765</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="default">763</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="string">&apos;egg&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">in_array</span><span class="keyword">(</span><span class="string">&apos;hhh&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">in_array</span><span class="keyword">(array(), </span><span class="default">$array</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// false<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re working with very large 2 dimensional arrays (eg 20,000+ elements) it&apos;s much faster to do this...
<br>
<br><span class="default">&lt;?php
<br>$needle </span><span class="keyword">= </span><span class="string">&apos;test for this&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">$flipped_haystack </span><span class="keyword">= </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$haystack</span><span class="keyword">);
<br>
<br>if ( isset(</span><span class="default">$flipped_haystack</span><span class="keyword">[</span><span class="default">$needle</span><span class="keyword">]) )
<br>{
<br>&#xA0; print </span><span class="string">&quot;Yes it&apos;s there!&quot;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>I had a script that went from 30+ seconds down to 2 seconds (when hunting through a 50,000 element array 50,000 times).
<br>
<br>Remember to only flip it once at the beginning of your code though!</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Method </span><span class="keyword">{<br><br>&#xA0; &#xA0; </span><span class="comment">/** @var int current number of inMultiArray() loop */<br>&#xA0; &#xA0; </span><span class="keyword">private static </span><span class="default">$currentMultiArrayExec </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Checks if an element is found in an array or one of its subarray.<br>&#xA0; &#xA0;&#xA0; * The verification layer of this method is limited to the parameter boundary xdebug.max_nesting_level of your php.ini.<br>&#xA0; &#xA0;&#xA0; * @param mixed $element<br>&#xA0; &#xA0;&#xA0; * @param array $array<br>&#xA0; &#xA0;&#xA0; * @param bool $strict<br>&#xA0; &#xA0;&#xA0; * @return bool<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public static function </span><span class="default">inMultiArray</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">, array </span><span class="default">$array</span><span class="keyword">, </span><span class="default">bool $strict </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">) : </span><span class="default">bool </span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$currentMultiArrayExec</span><span class="keyword">++;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">self</span><span class="keyword">::</span><span class="default">$currentMultiArrayExec </span><span class="keyword">&gt;= </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&quot;xdebug.max_nesting_level&quot;</span><span class="keyword">)) return </span><span class="default">false</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bool </span><span class="keyword">= </span><span class="default">$strict </span><span class="keyword">? </span><span class="default">$element </span><span class="keyword">=== </span><span class="default">$key </span><span class="keyword">: </span><span class="default">$element </span><span class="keyword">== </span><span class="default">$key</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$bool</span><span class="keyword">) return </span><span class="default">true</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bool </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">inMultiArray</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">, </span><span class="default">$strict</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bool </span><span class="keyword">= </span><span class="default">$strict </span><span class="keyword">? </span><span class="default">$element </span><span class="keyword">=== </span><span class="default">$value </span><span class="keyword">: </span><span class="default">$element </span><span class="keyword">== </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$bool</span><span class="keyword">) return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$currentMultiArrayExec </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return isset(</span><span class="default">$bool</span><span class="keyword">) ? </span><span class="default">$bool </span><span class="keyword">: </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$array </span><span class="keyword">= array(</span><span class="string">&quot;foo&quot; </span><span class="keyword">=&gt; array(</span><span class="string">&quot;foo2&quot;</span><span class="keyword">, </span><span class="string">&quot;bar&quot;</span><span class="keyword">));<br></span><span class="default">$search </span><span class="keyword">= </span><span class="string">&quot;foo&quot;</span><span class="keyword">;<br><br>if(</span><span class="default">Method</span><span class="keyword">::</span><span class="default">inMultiArray</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">)){<br>&#xA0; &#xA0; echo </span><span class="default">$search </span><span class="keyword">. </span><span class="string">&quot; it is found in the array or one of its sub array.&quot;</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; echo </span><span class="default">$search </span><span class="keyword">. </span><span class="string">&quot; was not found.&quot;</span><span class="keyword">;<br>}<br><br></span><span class="comment"># foo it is found in the array or one of its sub array.</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For a case-insensitive in_array(), you can use array_map() to avoid a foreach statement, e.g.:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">in_arrayi</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">, </span><span class="default">$haystack</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">in_array</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">), </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;strtolower&apos;</span><span class="keyword">, </span><span class="default">$haystack</span><span class="keyword">));<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Determine whether an object field matches needle.
<br>
<br>Usage Example:
<br>---------------
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= array( new </span><span class="default">stdClass</span><span class="keyword">(), new </span><span class="default">stdClass</span><span class="keyword">() );
<br></span><span class="default">$arr</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]-&gt;</span><span class="default">colour </span><span class="keyword">= </span><span class="string">&apos;red&apos;</span><span class="keyword">;
<br></span><span class="default">$arr</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]-&gt;</span><span class="default">colour </span><span class="keyword">= </span><span class="string">&apos;green&apos;</span><span class="keyword">;
<br></span><span class="default">$arr</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]-&gt;</span><span class="default">state&#xA0; </span><span class="keyword">= </span><span class="string">&apos;enabled&apos;</span><span class="keyword">;
<br>
<br>if (</span><span class="default">in_array_field</span><span class="keyword">(</span><span class="string">&apos;red&apos;</span><span class="keyword">, </span><span class="string">&apos;colour&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">))
<br>&#xA0;&#xA0; echo </span><span class="string">&apos;Item exists with colour red.&apos;</span><span class="keyword">;
<br>if (</span><span class="default">in_array_field</span><span class="keyword">(</span><span class="string">&apos;magenta&apos;</span><span class="keyword">, </span><span class="string">&apos;colour&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">))
<br>&#xA0;&#xA0; echo </span><span class="string">&apos;Item exists with colour magenta.&apos;</span><span class="keyword">;
<br>if (</span><span class="default">in_array_field</span><span class="keyword">(</span><span class="string">&apos;enabled&apos;</span><span class="keyword">, </span><span class="string">&apos;state&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">))
<br>&#xA0;&#xA0; echo </span><span class="string">&apos;Item exists with enabled state.&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Output:
<br>--------
<br>Item exists with colour red.
<br>Item exists with enabled state.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">in_array_field</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">, </span><span class="default">$needle_field</span><span class="keyword">, </span><span class="default">$haystack</span><span class="keyword">, </span><span class="default">$strict </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">) {
<br>&#xA0; &#xA0; if (</span><span class="default">$strict</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$haystack </span><span class="keyword">as </span><span class="default">$item</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$item</span><span class="keyword">-&gt;</span><span class="default">$needle_field</span><span class="keyword">) &amp;&amp; </span><span class="default">$item</span><span class="keyword">-&gt;</span><span class="default">$needle_field </span><span class="keyword">=== </span><span class="default">$needle</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$haystack </span><span class="keyword">as </span><span class="default">$item</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$item</span><span class="keyword">-&gt;</span><span class="default">$needle_field</span><span class="keyword">) &amp;&amp; </span><span class="default">$item</span><span class="keyword">-&gt;</span><span class="default">$needle_field </span><span class="keyword">== </span><span class="default">$needle</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you found yourself in need of a multidimensional array in_array like function you can use the one below. Works in a fair amount of time<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">in_multiarray</span><span class="keyword">(</span><span class="default">$elem</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$top </span><span class="keyword">= </span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bottom </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$bottom </span><span class="keyword">&lt;= </span><span class="default">$top</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$bottom</span><span class="keyword">] == </span><span class="default">$elem</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$bottom</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">in_multiarray</span><span class="keyword">(</span><span class="default">$elem</span><span class="keyword">, (</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$bottom</span><span class="keyword">])))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bottom</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.in-array.php)

**[To root](/README.md)**