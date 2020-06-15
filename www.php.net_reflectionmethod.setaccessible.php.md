# ReflectionMethod::setAccessible




<div class="phpcode"><span class="html">
This is handy for accessing private methods but remember that things are normally private for a reason! Unit Testing is one (debatable) use case for this.<br><br>Example:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0; private function </span><span class="default">myPrivateMethod</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">7</span><span class="keyword">;<br>&#xA0; }<br>}<br><br></span><span class="default">$method </span><span class="keyword">= new </span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="string">&apos;Foo&apos;</span><span class="keyword">, </span><span class="string">&apos;myPrivateMethod&apos;</span><span class="keyword">);<br></span><span class="default">$method</span><span class="keyword">-&gt;</span><span class="default">setAccessible</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br> <br>echo </span><span class="default">$method</span><span class="keyword">-&gt;</span><span class="default">invoke</span><span class="keyword">(new </span><span class="default">Foo</span><span class="keyword">);<br></span><span class="comment">// echos &quot;7&quot;<br></span><span class="default">?&gt;<br></span><br>This works nicely with PHPUnit: <a href="http://php.net/manual/en/reflectionmethod.setaccessible.php" rel="nofollow" target="_blank">http://php.net/manual/en/reflectionmethod.setaccessible.php</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.setaccessible.php)

**[To root](/README.md)**