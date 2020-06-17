# array_search




<div class="phpcode"><span class="html">
in (PHP 5 &gt;= 5.5.0) you don&apos;t have to write your own function to search through a multi dimensional array<br><br>ex : <br><br>$userdb=Array<br>(<br>&#xA0; &#xA0; (0) =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (uid) =&gt; &apos;100&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (name) =&gt; &apos;Sandra Shush&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (url) =&gt; &apos;urlof100&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br><br>&#xA0; &#xA0; (1) =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (uid) =&gt; &apos;5465&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (name) =&gt; &apos;Stefanie Mcmohn&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (pic_square) =&gt; &apos;urlof100&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br><br>&#xA0; &#xA0; (2) =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (uid) =&gt; &apos;40489&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (name) =&gt; &apos;Michael&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (pic_square) =&gt; &apos;urlof40489&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>);<br><br>simply u can use this<br><br>$key = array_search(40489, array_column($userdb, &apos;uid&apos;));</span>
</div>
  

#


<div class="phpcode"><span class="html">
About searcing in multi-dimentional arrays; two notes on &quot;xfoxawy at gmail dot com&quot;;<br><br>It perfectly searches through multi-dimentional arrays combined with array_column() (min php 5.5.0) but it may not return the values you&apos;d expect.<br><br><span class="default">&lt;?php array_search</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">, </span><span class="default">array_column</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="string">&apos;key&apos;</span><span class="keyword">)); </span><span class="default">?&gt;<br></span><br>Since array_column() will produce a resulting array; it won&apos;t preserve your multi-dimentional array&apos;s keys. So if you check against your keys, it will fail.<br><br>For example;<br><br><span class="default">&lt;?php<br>$people </span><span class="keyword">= array(<br>&#xA0; </span><span class="default">2 </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;John&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;fav_color&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;green&apos;<br>&#xA0; </span><span class="keyword">),<br>&#xA0; </span><span class="default">5</span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Samuel&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;fav_color&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;blue&apos;<br>&#xA0; </span><span class="keyword">)<br>);<br><br></span><span class="default">$found_key </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&apos;blue&apos;</span><span class="keyword">, </span><span class="default">array_column</span><span class="keyword">(</span><span class="default">$people</span><span class="keyword">, </span><span class="string">&apos;fav_color&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Here, you could expect that the $found_key would be &quot;5&quot; but it&apos;s NOT. It will be 1. Since it&apos;s the second element of the produced array by the array_column() function.<br><br>Secondly, if your array is big, I would recommend you to first assign a new variable so that it wouldn&apos;t call array_column() for each element it searches. For a better performance, you could do;<br><br><span class="default">&lt;?php<br>$colors </span><span class="keyword">= </span><span class="default">array_column</span><span class="keyword">(</span><span class="default">$people</span><span class="keyword">, </span><span class="string">&apos;fav_color&apos;</span><span class="keyword">);<br></span><span class="default">$found_key </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&apos;blue&apos;</span><span class="keyword">, </span><span class="default">$colors</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
$array = [&apos;a&apos;, &apos;b&apos;, &apos;c&apos;];<br>$key = array_search(&apos;a&apos;, $array); //$key = 0<br>if ($key) <br>{<br>//even a element is found in array, but if (0) means false<br>//...<br>}<br><br>//the correct way<br>if (false !== $key)<br>{<br>//....<br>}<br><br>It&apos;s what the document stated &quot;may also return a non-Boolean value which evaluates to FALSE.&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
the recursive function by tony have a small bug. it failes when a key is 0<br><br>here is the corrected version of this helpful function:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">recursive_array_search</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">,</span><span class="default">$haystack</span><span class="keyword">) {<br>&#xA0; &#xA0; foreach(</span><span class="default">$haystack </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$current_key</span><span class="keyword">=</span><span class="default">$key</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$needle</span><span class="keyword">===</span><span class="default">$value </span><span class="keyword">OR (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">) &amp;&amp; </span><span class="default">recursive_array_search</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">,</span><span class="default">$value</span><span class="keyword">) !== </span><span class="default">false</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$current_key</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are using the result of array_search in a condition statement, make sure you use the === operator instead of == to test whether or not it found a match.&#xA0; Otherwise, searching through an array with numeric indicies will result in index 0 always getting evaluated as false/null.&#xA0; This nuance cost me a lot of time and sanity, so I hope this helps someone.&#xA0; In case you don&apos;t know what I&apos;m talking about, here&apos;s an example:
<br>
<br><span class="default">&lt;?php
<br>$code </span><span class="keyword">= array(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="string">&quot;b&quot;</span><span class="keyword">, </span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="string">&quot;c&quot;</span><span class="keyword">, </span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="string">&quot;b&quot;</span><span class="keyword">, </span><span class="string">&quot;b&quot;</span><span class="keyword">); </span><span class="comment">// infamous abacabb mortal kombat code :-P
<br>
<br>// this is WRONG
<br></span><span class="keyword">while ((</span><span class="default">$key </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">$code</span><span class="keyword">)) != </span><span class="default">NULL</span><span class="keyword">)
<br>{
<br> </span><span class="comment">// infinite loop, regardless of the unset
<br> </span><span class="keyword">unset(</span><span class="default">$code</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);
<br>}
<br>
<br></span><span class="comment">// this is _RIGHT_
<br></span><span class="keyword">while ((</span><span class="default">$key </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">$code</span><span class="keyword">)) !== </span><span class="default">NULL</span><span class="keyword">)
<br>{
<br> </span><span class="comment">// loop will terminate
<br> </span><span class="keyword">unset(</span><span class="default">$code</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
for searching case insensitive better this:
<br>
<br><span class="default">&lt;?php
<br>array_search</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">),</span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;strtolower&apos;</span><span class="keyword">,</span><span class="default">$array</span><span class="keyword">));
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To expand on previous comments, here are some examples of<br>where using array_search within an IF statement can go<br>wrong when you want to use the array key thats returned.<br><br>Take the following two arrays you wish to search:<br><br><span class="default">&lt;?php<br>$fruit_array </span><span class="keyword">= array(</span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="string">&quot;pear&quot;</span><span class="keyword">, </span><span class="string">&quot;orange&quot;</span><span class="keyword">);<br></span><span class="default">$fruit_array </span><span class="keyword">= array(</span><span class="string">&quot;a&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="string">&quot;b&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;pear&quot;</span><span class="keyword">, </span><span class="string">&quot;c&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;orange&quot;</span><span class="keyword">);<br><br>if (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="default">$fruit_array</span><span class="keyword">))<br></span><span class="comment">//PROBLEM: the first array returns a key of 0 and IF treats it as FALSE<br><br></span><span class="keyword">if (</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="default">$fruit_array</span><span class="keyword">)))<br></span><span class="comment">//PROBLEM: works on numeric keys of the first array but fails on the second<br><br></span><span class="keyword">if (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="default">$fruit_array</span><span class="keyword">)))<br></span><span class="comment">//PROBLEM: using the above in the wrong order causes $i to always equal 1<br><br></span><span class="keyword">if (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="default">$fruit_array</span><span class="keyword">) !== </span><span class="default">FALSE</span><span class="keyword">)<br></span><span class="comment">//PROBLEM: explicit with no extra brackets causes $i to always equal 1<br><br></span><span class="keyword">if ((</span><span class="default">$i </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="default">$fruit_array</span><span class="keyword">)) !== </span><span class="default">FALSE</span><span class="keyword">)<br></span><span class="comment">//YES: works on both arrays returning their keys<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
hey i have a easy multidimensional array search function 
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">search</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$results </span><span class="keyword">= array();
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]) &amp;&amp; </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] == </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$results</span><span class="keyword">[] = </span><span class="default">$array</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$subarray</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$results </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$results</span><span class="keyword">, </span><span class="default">search</span><span class="keyword">(</span><span class="default">$subarray</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$results</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Better solution of multidimensional searching.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">multidimensional_search</span><span class="keyword">(</span><span class="default">$parents</span><span class="keyword">, </span><span class="default">$searched</span><span class="keyword">) {
<br>&#xA0; if (empty(</span><span class="default">$searched</span><span class="keyword">) || empty(</span><span class="default">$parents</span><span class="keyword">)) {
<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; }
<br> 
<br>&#xA0; foreach (</span><span class="default">$parents </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$exists </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; foreach (</span><span class="default">$searched </span><span class="keyword">as </span><span class="default">$skey </span><span class="keyword">=&gt; </span><span class="default">$svalue</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$exists </span><span class="keyword">= (</span><span class="default">$exists </span><span class="keyword">&amp;&amp; IsSet(</span><span class="default">$parents</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">][</span><span class="default">$skey</span><span class="keyword">]) &amp;&amp; </span><span class="default">$parents</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">][</span><span class="default">$skey</span><span class="keyword">] == </span><span class="default">$svalue</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(</span><span class="default">$exists</span><span class="keyword">){ return </span><span class="default">$key</span><span class="keyword">; }
<br>&#xA0; }
<br> 
<br>&#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">$parents </span><span class="keyword">= array();
<br></span><span class="default">$parents</span><span class="keyword">[] = array(</span><span class="string">&apos;date&apos;</span><span class="keyword">=&gt;</span><span class="default">1320883200</span><span class="keyword">, </span><span class="string">&apos;uid&apos;</span><span class="keyword">=&gt;</span><span class="default">3</span><span class="keyword">);
<br></span><span class="default">$parents</span><span class="keyword">[] = array(</span><span class="string">&apos;date&apos;</span><span class="keyword">=&gt;</span><span class="default">1320883200</span><span class="keyword">, </span><span class="string">&apos;uid&apos;</span><span class="keyword">=&gt;</span><span class="default">5</span><span class="keyword">);
<br></span><span class="default">$parents</span><span class="keyword">[] = array(</span><span class="string">&apos;date&apos;</span><span class="keyword">=&gt;</span><span class="default">1318204800</span><span class="keyword">, </span><span class="string">&apos;uid&apos;</span><span class="keyword">=&gt;</span><span class="default">5</span><span class="keyword">);
<br>
<br>echo </span><span class="default">multidimensional_search</span><span class="keyword">(</span><span class="default">$parents</span><span class="keyword">, array(</span><span class="string">&apos;date&apos;</span><span class="keyword">=&gt;</span><span class="default">1320883200</span><span class="keyword">, </span><span class="string">&apos;uid&apos;</span><span class="keyword">=&gt;</span><span class="default">5</span><span class="keyword">)); </span><span class="comment">// 1
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-search.php)

**[To root](/README.md)**