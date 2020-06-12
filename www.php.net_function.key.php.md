# key




<div class="phpcode"><span class="html">
Note that using key($array) in a foreach loop may have unexpected results.&#xA0; <br><br>When requiring the key inside a foreach loop, you should use:<br>foreach($array as $key =&gt; $value)<br><br>I was incorrectly using:<br><span class="default">&lt;?php<br></span><span class="keyword">foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$value</span><span class="keyword">)<br>{<br>&#xA0; </span><span class="default">$mykey </span><span class="keyword">= </span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>and experiencing errors (the pointer of the array is already moved to the next item, so instead of getting the key for $value, you will get the key to the next value in the array)<br><br>CORRECT:<br><span class="default">&lt;?php<br></span><span class="keyword">foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)<br>{<br>&#xA0; </span><span class="default">$mykey </span><span class="keyword">= </span><span class="default">$key</span><span class="keyword">;<br>}<br><br></span><span class="default">A noob error</span><span class="keyword">, </span><span class="default">but felt it might help someone </span><span class="keyword">else </span><span class="default">out there</span><span class="keyword">.</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Suppose if the array values are in numbers and numbers contains `0` then the loop will be terminated. To overcome this you can user like this<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;0&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;5&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;1&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;2&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;2&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;0&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;3&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;3&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;4&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;1&apos;</span><span class="keyword">);<br><br></span><span class="comment">// wrong approach<br><br></span><span class="keyword">while (</span><span class="default">$fruit_name </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">).</span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">next</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br>}<br><br></span><span class="comment">// the way will be break loop when arra(&apos;2&apos;=&gt;0) because its value is &apos;0&apos;, while(0) will terminate the loop<br><br>// correct approach<br></span><span class="keyword">while ( (</span><span class="default">$fruit_name </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)) !== </span><span class="default">FALSE </span><span class="keyword">) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">).</span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">next</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br>}<br></span><span class="comment">//this will work properly<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Needed to get the index of the max/highest value in an assoc array.<br>max() only returned the value, no index, so I did this instead.<br><br><span class="default">&lt;?php<br>reset</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">// optional.<br></span><span class="default">arsort</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">);<br></span><span class="default">$key_of_max </span><span class="keyword">= </span><span class="default">key</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">// returns the index.<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.key.php)

**[⬆ to root](/)**