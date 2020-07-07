# get_class_methods




<div class="phpcode"><span class="html">
It should be noted that the returned methods are dependant on the current scope. See this example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">C<br></span><span class="keyword">{<br>&#xA0; &#xA0; private function </span><span class="default">privateMethod</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">publicMethod</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;$this:&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">get_class_methods</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;C (inside class):&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">get_class_methods</span><span class="keyword">(</span><span class="string">&apos;C&apos;</span><span class="keyword">));<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">$c </span><span class="keyword">= new </span><span class="default">C</span><span class="keyword">;<br>echo </span><span class="string">&apos;$c:&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">get_class_methods</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">));<br>echo </span><span class="string">&apos;C (outside class):&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">get_class_methods</span><span class="keyword">(</span><span class="string">&apos;C&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Output:<br><br>$this:<br>array<br>&#xA0; 0 =&gt; string &apos;privateMethod&apos; (length=13)<br>&#xA0; 1 =&gt; string &apos;publicMethod&apos; (length=12)<br>&#xA0; 2 =&gt; string &apos;__construct&apos; (length=11)<br><br>C (inside class):<br>array<br>&#xA0; 0 =&gt; string &apos;privateMethod&apos; (length=13)<br>&#xA0; 1 =&gt; string &apos;publicMethod&apos; (length=12)<br>&#xA0; 2 =&gt; string &apos;__construct&apos; (length=11)<br><br>$c:<br>array<br>&#xA0; 0 =&gt; string &apos;publicMethod&apos; (length=12)<br>&#xA0; 1 =&gt; string &apos;__construct&apos; (length=11)<br><br>C (outside class):<br>array<br>&#xA0; 0 =&gt; string &apos;publicMethod&apos; (length=12)<br>&#xA0; 1 =&gt; string &apos;__construct&apos; (length=11)</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is important to note that get_class_methods($class) returns not only methods defined by $class but also the inherited methods.<br><br>There does not appear to be any PHP function to determine which methods are inherited and which are defined explicitly by a class.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-class-methods.php)

**[To root](/README.md)**