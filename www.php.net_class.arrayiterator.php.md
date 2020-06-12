# The ArrayIterator class




<div class="phpcode"><span class="html">
Another fine Iterator from php . You can use it especially when you have to iterate over objects<br><br><span class="default">&lt;?php<br>$fruits </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&quot;apple&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;yummy&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;orange&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;ah ya, nice&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;grape&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;wow, I love it!&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;plum&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;nah, not me&quot;<br></span><span class="keyword">);<br></span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">ArrayObject</span><span class="keyword">( </span><span class="default">$fruits </span><span class="keyword">);<br></span><span class="default">$it </span><span class="keyword">= </span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">getIterator</span><span class="keyword">();<br><br></span><span class="comment">// How many items are we iterating over?<br><br></span><span class="keyword">echo </span><span class="string">&quot;Iterating over: &quot; </span><span class="keyword">. </span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">count</span><span class="keyword">() . </span><span class="string">&quot; values\n&quot;</span><span class="keyword">;<br><br></span><span class="comment">// Iterate over the values in the ArrayObject:<br></span><span class="keyword">while( </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">valid</span><span class="keyword">() )<br>{<br>&#xA0; &#xA0; echo </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">key</span><span class="keyword">() . </span><span class="string">&quot;=&quot; </span><span class="keyword">. </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">() . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">();<br>}<br><br></span><span class="comment">// The good thing here is that it can be iterated with foreach loop<br><br></span><span class="keyword">foreach (</span><span class="default">$it </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$val</span><span class="keyword">)<br>echo </span><span class="default">$key</span><span class="keyword">.</span><span class="string">&quot;:&quot;</span><span class="keyword">.</span><span class="default">$val</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="comment">/* Outputs something like */<br><br></span><span class="default">Iterating over</span><span class="keyword">: </span><span class="default">4 values<br>apple</span><span class="keyword">=</span><span class="default">yummy<br>orange</span><span class="keyword">=</span><span class="default">ah ya</span><span class="keyword">, </span><span class="default">nice<br>grape</span><span class="keyword">=</span><span class="default">wow</span><span class="keyword">, </span><span class="default">I love it</span><span class="keyword">!<br></span><span class="default">plum</span><span class="keyword">=</span><span class="default">nah</span><span class="keyword">, </span><span class="default">not me<br><br>?&gt;<br></span><br>Regards.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Need a callback on an iterated value, but don&apos;t have PHP 5.4+?&#xA0; This makes is stupid easy:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">ArrayCallbackIterator </span><span class="keyword">extends </span><span class="default">ArrayIterator </span><span class="keyword">{
<br>&#xA0; private </span><span class="default">$callback</span><span class="keyword">;
<br>&#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$callback</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">callback </span><span class="keyword">= </span><span class="default">$callback</span><span class="keyword">;
<br>&#xA0; }
<br>
<br>&#xA0; public function </span><span class="default">current</span><span class="keyword">() {
<br>&#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">parent</span><span class="keyword">::</span><span class="default">current</span><span class="keyword">();
<br>&#xA0; &#xA0; return </span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">callback</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">);
<br>&#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>You can use it pretty much exactly as the Array Iterator:
<br>
<br><span class="default">&lt;?php
<br>$iterator1 </span><span class="keyword">= new </span><span class="default">ArrayCallbackIterator</span><span class="keyword">(</span><span class="default">$valueList</span><span class="keyword">, </span><span class="string">&quot;callback_function&quot;</span><span class="keyword">);
<br></span><span class="default">$iterator2 </span><span class="keyword">= new </span><span class="default">ArrayCallbackIterator</span><span class="keyword">(</span><span class="default">$valueList</span><span class="keyword">, array(</span><span class="default">$object</span><span class="keyword">, </span><span class="string">&quot;callback_class_method&quot;</span><span class="keyword">));
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.arrayiterator.php)

**[⬆ to root](/)**