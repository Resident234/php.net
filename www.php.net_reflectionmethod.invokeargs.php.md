# ReflectionMethod::invokeArgs




<div class="phpcode"><span class="html">
We can do black magic, which is useful in templating block calls:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0;&#xA0; $object</span><span class="keyword">-&gt;</span><span class="default">__named</span><span class="keyword">(</span><span class="string">&apos;methodNameHere&apos;</span><span class="keyword">, array(</span><span class="string">&apos;arg3&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;three&apos;</span><span class="keyword">, </span><span class="string">&apos;arg1&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;one&apos;</span><span class="keyword">));
<br>
<br>&#xA0; &#xA0;&#xA0; ...
<br>
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">/**
<br>&#xA0; &#xA0; &#xA0;&#xA0; * Pass method arguments by name
<br>&#xA0; &#xA0; &#xA0;&#xA0; *
<br>&#xA0; &#xA0; &#xA0;&#xA0; * @param string $method
<br>&#xA0; &#xA0; &#xA0;&#xA0; * @param array $args
<br>&#xA0; &#xA0; &#xA0;&#xA0; * @return mixed
<br>&#xA0; &#xA0; &#xA0;&#xA0; */
<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__named</span><span class="keyword">(</span><span class="default">$method</span><span class="keyword">, array </span><span class="default">$args </span><span class="keyword">= array())
<br>&#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$reflection </span><span class="keyword">= new </span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, </span><span class="default">$method</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pass </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$reflection</span><span class="keyword">-&gt;</span><span class="default">getParameters</span><span class="keyword">() as </span><span class="default">$param</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* @var $param ReflectionParameter */
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(isset(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">$param</span><span class="keyword">-&gt;</span><span class="default">getName</span><span class="keyword">()]))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pass</span><span class="keyword">[] = </span><span class="default">$args</span><span class="keyword">[</span><span class="default">$param</span><span class="keyword">-&gt;</span><span class="default">getName</span><span class="keyword">()];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pass</span><span class="keyword">[] = </span><span class="default">$param</span><span class="keyword">-&gt;</span><span class="default">getDefaultValue</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$reflection</span><span class="keyword">-&gt;</span><span class="default">invokeArgs</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, </span><span class="default">$pass</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.invokeargs.php)

**[⬆ to root](/)**