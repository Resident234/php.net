# get_class_vars




<div class="phpcode"><span class="html">
If you want to retrieve the class vars from within the class itself, use $this.<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br><br>&#xA0; &#xA0; var </span><span class="default">$a</span><span class="keyword">;<br>&#xA0; &#xA0; var </span><span class="default">$b</span><span class="keyword">;<br>&#xA0; &#xA0; var </span><span class="default">$c</span><span class="keyword">;<br>&#xA0; &#xA0; var </span><span class="default">$d</span><span class="keyword">;<br>&#xA0; &#xA0; var </span><span class="default">$e</span><span class="keyword">;<br><br>&#xA0; &#xA0; function </span><span class="default">GetClassVars</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">get_class_vars</span><span class="keyword">(</span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">))); </span><span class="comment">// $this<br>&#xA0; &#xA0; </span><span class="keyword">}<br><br>}<br><br></span><span class="default">$Foo </span><span class="keyword">= new </span><span class="default">Foo</span><span class="keyword">;<br><br></span><span class="default">$class_vars </span><span class="keyword">= </span><span class="default">$Foo</span><span class="keyword">-&gt;</span><span class="default">GetClassVars</span><span class="keyword">();<br><br>foreach (</span><span class="default">$class_vars </span><span class="keyword">as </span><span class="default">$cvar</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; echo </span><span class="default">$cvar </span><span class="keyword">. </span><span class="string">&quot;&lt;br /&gt;\n&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Produces, after PHP 4.2.0, the following:<br><br>a<br>b<br>c<br>d<br>e</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-class-vars.php)

**[To root](/README.md)**