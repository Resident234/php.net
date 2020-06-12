# end




<div class="phpcode"><span class="html">
It&apos;s interesting to note that when creating an array with numeric keys in no particular order, end() will still only return the value that was the last one to be created. So, if you have something like this:
<br>
<br>&#xA0; &#xA0; <span class="default">&lt;?php
<br>&#xA0; &#xA0; $a </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$a</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$a</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; echo </span><span class="default">end</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">?&gt;
<br></span>
<br>This will print &quot;0&quot;.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need to get a reference on the first or last element of an array, use these functions because reset() and end() only return you a copy that you cannot dereference directly:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">first</span><span class="keyword">(&amp;</span><span class="default">$array</span><span class="keyword">) {
<br>if (!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)) return &amp;</span><span class="default">$array</span><span class="keyword">;
<br>if (!</span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)) return </span><span class="default">null</span><span class="keyword">;
<br></span><span class="default">reset</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);
<br>return &amp;</span><span class="default">$array</span><span class="keyword">[</span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)];
<br>}
<br>
<br>function </span><span class="default">last</span><span class="keyword">(&amp;</span><span class="default">$array</span><span class="keyword">) {
<br>if (!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)) return &amp;</span><span class="default">$array</span><span class="keyword">;
<br>if (!</span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)) return </span><span class="default">null</span><span class="keyword">;
<br></span><span class="default">end</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);
<br>return &amp;</span><span class="default">$array</span><span class="keyword">[</span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)];
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function returns the value at the end of the array, but you may sometimes be interested in the key at the end of the array, particularly when working with non integer indexed arrays:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Returns the key at the end of the array<br></span><span class="keyword">function </span><span class="default">endKey</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">){<br> </span><span class="default">end</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br> return </span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Usage example:<br><span class="default">&lt;?php<br>$a </span><span class="keyword">= array(</span><span class="string">&quot;one&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;apple&quot;</span><span class="keyword">, </span><span class="string">&quot;two&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;orange&quot;</span><span class="keyword">, </span><span class="string">&quot;three&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;pear&quot;</span><span class="keyword">);<br>echo </span><span class="default">endKey</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">); </span><span class="comment">// will output &quot;three&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If all you want is the last item of the array without affecting the internal array pointer just do the following:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">endc</span><span class="keyword">( </span><span class="default">$array </span><span class="keyword">) { return </span><span class="default">end</span><span class="keyword">( </span><span class="default">$array </span><span class="keyword">); }<br><br></span><span class="default">$items </span><span class="keyword">= array( </span><span class="string">&apos;one&apos;</span><span class="keyword">, </span><span class="string">&apos;two&apos;</span><span class="keyword">, </span><span class="string">&apos;three&apos; </span><span class="keyword">);<br></span><span class="default">$lastItem </span><span class="keyword">= </span><span class="default">endc</span><span class="keyword">( </span><span class="default">$items </span><span class="keyword">); </span><span class="comment">// three<br></span><span class="default">$current </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">( </span><span class="default">$items </span><span class="keyword">); </span><span class="comment">// one<br></span><span class="default">?&gt;<br></span><br>This works because the parameter to the function is being sent as a copy, not as a reference to the original variable.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.end.php)

**[⬆ to root](/)**