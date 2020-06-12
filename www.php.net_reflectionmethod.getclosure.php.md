# ReflectionMethod::getClosure




<div class="phpcode"><span class="html">
You can call private methods with getClosure():<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">call_private_method</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">, </span><span class="default">$method</span><span class="keyword">, </span><span class="default">$args </span><span class="keyword">= array()) {<br>&#xA0; &#xA0; </span><span class="default">$reflection </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">));<br>&#xA0; &#xA0; </span><span class="default">$closure </span><span class="keyword">= </span><span class="default">$reflection</span><span class="keyword">-&gt;</span><span class="default">getMethod</span><span class="keyword">(</span><span class="default">$method</span><span class="keyword">)-&gt;</span><span class="default">getClosure</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">call_user_func_array</span><span class="keyword">(</span><span class="default">$closure</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">);<br>}<br><br>class </span><span class="default">Example </span><span class="keyword">{<br><br>&#xA0; &#xA0; private </span><span class="default">$x </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">, </span><span class="default">$y </span><span class="keyword">= </span><span class="default">10</span><span class="keyword">;<br><br>&#xA0; &#xA0; private function </span><span class="default">sum</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">x </span><span class="keyword">+ </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">y</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">call_private_method</span><span class="keyword">(new </span><span class="default">Example</span><span class="keyword">(), </span><span class="string">&apos;sum&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Output is 11.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.getclosure.php)

**[⬆ to root](/)**