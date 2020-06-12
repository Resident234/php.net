# ReflectionClass::getDocComment




<div class="phpcode"><span class="html">
According to what I can find in the PHP (5.3.2) source code, getDocComment will return the doc comment as the parser found it.<br>The doc comment (T_DOC_COMMENT) must begin with a /** - that&apos;s two asterisks, not one. The comment continues until the first */. A normal multi-line comment /*...*/ (T_COMMENT) does not count as a doc comment.<br><br>The doc comment itself includes those five characters, so <span class="default">&lt;?php substr</span><span class="keyword">(</span><span class="default">$doccomment</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, -</span><span class="default">2</span><span class="keyword">) </span><span class="default">?&gt;</span> will get you what&apos;s inside. A call to trim() after is recommended.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re using a bytecode cache like eAccelerator this method will return FALSE even if there is a properly formatted Docblock. It looks like the information required by this method gets stripped out by the bytecode cache.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can also get the docblock definitions for the defined methods of a class as such:<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * This is an Example class<br> */<br></span><span class="keyword">class </span><span class="default">Example<br></span><span class="keyword">{<br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * This is an example function<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">fn</span><span class="keyword">() <br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// void<br>&#xA0; &#xA0; </span><span class="keyword">}<br>}<br><br></span><span class="default">$reflector </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;Example&apos;</span><span class="keyword">);<br><br></span><span class="comment">// to get the Class DocBlock<br></span><span class="keyword">echo </span><span class="default">$reflector</span><span class="keyword">-&gt;</span><span class="default">getDocComment</span><span class="keyword">()<br><br></span><span class="comment">// to get the Method DocBlock<br></span><span class="default">$reflector</span><span class="keyword">-&gt;</span><span class="default">getMethod</span><span class="keyword">(</span><span class="string">&apos;fn&apos;</span><span class="keyword">)-&gt;</span><span class="default">getDocComment</span><span class="keyword">();<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getdoccomment.php)

**[⬆ to root](/)**