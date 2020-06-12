# Sorting Arrays




<div class="phpcode"><span class="html">
While this may seem obvious, user-defined array sorting functions ( uksort(), uasort(), usort() ) will *not* be called if the array does not have *at least two values in it*.<br><br>The following code:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">usortTest</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">);<br>&#xA0; &#xA0; return -</span><span class="default">1</span><span class="keyword">;<br>}<br><br></span><span class="default">$test </span><span class="keyword">= array(</span><span class="string">&apos;val1&apos;</span><span class="keyword">);<br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$test</span><span class="keyword">, </span><span class="string">&quot;usortTest&quot;</span><span class="keyword">);<br><br></span><span class="default">$test2 </span><span class="keyword">= array(</span><span class="string">&apos;val2&apos;</span><span class="keyword">, </span><span class="string">&apos;val3&apos;</span><span class="keyword">);<br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$test2</span><span class="keyword">, </span><span class="string">&quot;usortTest&quot;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Will output: <br><br>string(4) &quot;val3&quot;<br>string(4) &quot;val2&quot;<br><br>The first array doesn&apos;t get sent to the function.<br><br>Please, under no circumstance, place any logic that modifies values, or applies non-sorting business logic in these functions as they will not always be executed.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Another way to do a case case-insensitive sort by key would simply be:<br><br><span class="default">&lt;?php<br>uksort</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="string">&apos;strcasecmp&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Since strcasecmp is already predefined in php it saves you the trouble to actually write the comparison function yourself.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/array.sorting.php)

**[⬆ to root](/)**