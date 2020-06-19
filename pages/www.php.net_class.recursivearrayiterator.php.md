# The RecursiveArrayIterator class




<div class="phpcode"><span class="html">
If you are iterating over a multi-dimensional array of objects, you may be tempted to use a RecursiveArrayIterator within a RecursiveIteratorIterator. You are likely to get baffling results if you do. That is because RecursiveArrayIterator treats all objects as having children, and tries to recurse into them. But if you are interested in having your RecursiveIteratorIterator return the objects in your multi-dimensional array, then you don&apos;t want the default setting LEAVES_ONLY, because no object can be a leaf (= has no children).<br><br>The solution is to extend the RecursiveArrayIterator class and override the hasChildren method appropriately. Something like the following might be suitable:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">RecursiveArrayOnlyIterator </span><span class="keyword">extends </span><span class="default">RecursiveArrayIterator </span><span class="keyword">{<br>&#xA0; public function </span><span class="default">hasChildren</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">());<br>&#xA0; }<br>}<br></span><span class="default">?&gt;<br></span>Of course, this simple example will not recurse into ArrayObjects either!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using the RecursiveArrayIterator to traverse an unknown amount of sub arrays within the outer array. Note: This functionality is already provided by using the RecursiveIteratorIterator but is useful in understanding how to use the iterator when using for the first time as all the terminology does get rather confusing at first sight of SPL!
<br>
<br><span class="default">&lt;?php
<br>$myArray </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&apos;a&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="default">1 </span><span class="keyword">=&gt; array(</span><span class="string">&apos;subA&apos;</span><span class="keyword">,</span><span class="string">&apos;subB&apos;</span><span class="keyword">,array(</span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&apos;subsubA&apos;</span><span class="keyword">, </span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&apos;subsubB&apos;</span><span class="keyword">, </span><span class="default">2 </span><span class="keyword">=&gt; array(</span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&apos;deepA&apos;</span><span class="keyword">, </span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&apos;deepB&apos;</span><span class="keyword">))),
<br>&#xA0; &#xA0; </span><span class="default">2 </span><span class="keyword">=&gt; </span><span class="string">&apos;b&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="default">3 </span><span class="keyword">=&gt; array(</span><span class="string">&apos;subA&apos;</span><span class="keyword">,</span><span class="string">&apos;subB&apos;</span><span class="keyword">,</span><span class="string">&apos;subC&apos;</span><span class="keyword">),
<br>&#xA0; &#xA0; </span><span class="default">4 </span><span class="keyword">=&gt; </span><span class="string">&apos;c&apos;
<br></span><span class="keyword">);
<br>
<br></span><span class="default">$iterator </span><span class="keyword">= new </span><span class="default">RecursiveArrayIterator</span><span class="keyword">(</span><span class="default">$myArray</span><span class="keyword">);
<br></span><span class="default">iterator_apply</span><span class="keyword">(</span><span class="default">$iterator</span><span class="keyword">, </span><span class="string">&apos;traverseStructure&apos;</span><span class="keyword">, array(</span><span class="default">$iterator</span><span class="keyword">));
<br>
<br>function </span><span class="default">traverseStructure</span><span class="keyword">(</span><span class="default">$iterator</span><span class="keyword">) {
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; while ( </span><span class="default">$iterator </span><span class="keyword">-&gt; </span><span class="default">valid</span><span class="keyword">() ) {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">$iterator </span><span class="keyword">-&gt; </span><span class="default">hasChildren</span><span class="keyword">() ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">traverseStructure</span><span class="keyword">(</span><span class="default">$iterator </span><span class="keyword">-&gt; </span><span class="default">getChildren</span><span class="keyword">());
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$iterator </span><span class="keyword">-&gt; </span><span class="default">key</span><span class="keyword">() . </span><span class="string">&apos; : &apos; </span><span class="keyword">. </span><span class="default">$iterator </span><span class="keyword">-&gt; </span><span class="default">current</span><span class="keyword">() .</span><span class="default">PHP_EOL</span><span class="keyword">;&#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$iterator </span><span class="keyword">-&gt; </span><span class="default">next</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>The output from which is:
<br>0 : a
<br>0 : subA
<br>1 : subB
<br>0 : subsubA
<br>1 : subsubB
<br>0 : deepA
<br>1 : deepB
<br>2 : b
<br>0 : subA
<br>1 : subB
<br>2 : subC
<br>4 : c</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.recursivearrayiterator.php)

**[To root](/README.md)**