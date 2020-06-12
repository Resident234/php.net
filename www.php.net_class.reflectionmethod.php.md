# The ReflectionMethod class




<div class="phpcode"><span class="html">
Note that the public member $class contains the name of the class in which the method has been defined:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{public function </span><span class="default">__construct</span><span class="keyword">() {}}<br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A </span><span class="keyword">{}<br><br></span><span class="default">$method </span><span class="keyword">= new </span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="string">&apos;B&apos;</span><span class="keyword">, </span><span class="string">&apos;__construct&apos;</span><span class="keyword">);<br>echo </span><span class="default">$method</span><span class="keyword">-&gt;</span><span class="default">class</span><span class="keyword">; </span><span class="comment">// prints &apos;A&apos;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.reflectionmethod.php)

**[⬆ to root](/)**