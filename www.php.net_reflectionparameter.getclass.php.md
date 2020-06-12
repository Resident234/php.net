# ReflectionParameter::getClass




<div class="phpcode"><span class="html">
The method returns ReflectionClass object of parameter type class or NULL if none.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{<br>&#xA0; &#xA0; function </span><span class="default">b</span><span class="keyword">(</span><span class="default">B $c</span><span class="keyword">, array </span><span class="default">$d</span><span class="keyword">, </span><span class="default">$e</span><span class="keyword">) {<br>&#xA0; &#xA0; }<br>}<br>class </span><span class="default">B </span><span class="keyword">{<br>}<br><br></span><span class="default">$refl </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;A&apos;</span><span class="keyword">);<br></span><span class="default">$par </span><span class="keyword">= </span><span class="default">$refl</span><span class="keyword">-&gt;</span><span class="default">getMethod</span><span class="keyword">(</span><span class="string">&apos;b&apos;</span><span class="keyword">)-&gt;</span><span class="default">getParameters</span><span class="keyword">();<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$par</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]-&gt;</span><span class="default">getClass</span><span class="keyword">()-&gt;</span><span class="default">getName</span><span class="keyword">());&#xA0; </span><span class="comment">// outputs B<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$par</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]-&gt;</span><span class="default">getClass</span><span class="keyword">());&#xA0; </span><span class="comment">// note that array type outputs NULL<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$par</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]-&gt;</span><span class="default">getClass</span><span class="keyword">());&#xA0; </span><span class="comment">// outputs NULL<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionparameter.getclass.php)

**[⬆ to root](/)**