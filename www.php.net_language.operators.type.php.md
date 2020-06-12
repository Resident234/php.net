# Type Operators




<div class="phpcode"><span class="html">
Posting this so the word typeof appears on this page, so that this page will show up when you google &apos;php typeof&apos;.&#xA0; ...yeah, former Java user.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You are also able to compare 2 objects using instanceOf. In that case, instanceOf will compare the types of both objects. That is sometimes very useful:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{ }<br>class </span><span class="default">B </span><span class="keyword">{ }<br><br></span><span class="default">$a </span><span class="keyword">= new </span><span class="default">A</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">B</span><span class="keyword">;<br></span><span class="default">$a2 </span><span class="keyword">= new </span><span class="default">A</span><span class="keyword">;<br><br>echo </span><span class="default">$a </span><span class="keyword">instanceOf </span><span class="default">$a</span><span class="keyword">; </span><span class="comment">// true<br></span><span class="keyword">echo </span><span class="default">$a </span><span class="keyword">instanceOf </span><span class="default">$b</span><span class="keyword">; </span><span class="comment">// false<br></span><span class="keyword">echo </span><span class="default">$a </span><span class="keyword">instanceOf </span><span class="default">$a2</span><span class="keyword">; </span><span class="comment">// true<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Checking an object is not an instance of a class, example #3 uses extraneous parentheses.<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(!(</span><span class="default">$a </span><span class="keyword">instanceof </span><span class="default">stdClass</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Because instanceof has higher operator precedence than ! you can just do<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">( ! </span><span class="default">$a </span><span class="keyword">instanceof </span><span class="default">stdClass </span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I don&apos;t see any mention of &quot;namespaces&quot; on this page so I thought I would chime in. The instanceof operator takes FQCN as second operator when you pass it as string and not a simple class name. It will not resolve it even if you have a `use MyNamespace\Bar;` at the top level. Here is what I am trying to say:<br><br>## testinclude.php ##<br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">Bar1</span><span class="keyword">;<br>{<br>class </span><span class="default">Foo1</span><span class="keyword">{ }<br>}<br>namespace </span><span class="default">Bar2</span><span class="keyword">;<br>{<br>class </span><span class="default">Foo2</span><span class="keyword">{ }<br>}<br></span><span class="default">?&gt;<br></span>## test.php ##<br><span class="default">&lt;?php<br></span><span class="keyword">include(</span><span class="string">&apos;testinclude.php&apos;</span><span class="keyword">);<br>use </span><span class="default">Bar1</span><span class="keyword">\</span><span class="default">Foo1 </span><span class="keyword">as </span><span class="default">Foo</span><span class="keyword">;<br></span><span class="default">$foo1 </span><span class="keyword">= new </span><span class="default">Foo</span><span class="keyword">(); </span><span class="default">$className </span><span class="keyword">= </span><span class="string">&apos;Bar1\Foo1&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo1 </span><span class="keyword">instanceof </span><span class="default">Bar1</span><span class="keyword">\</span><span class="default">Foo1</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo1 </span><span class="keyword">instanceof </span><span class="default">$className</span><span class="keyword">);<br></span><span class="default">$className </span><span class="keyword">= </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo1 </span><span class="keyword">instanceof </span><span class="default">$className</span><span class="keyword">);<br>use </span><span class="default">Bar2</span><span class="keyword">\</span><span class="default">Foo2</span><span class="keyword">;<br></span><span class="default">$foo2 </span><span class="keyword">= new </span><span class="default">Foo2</span><span class="keyword">(); </span><span class="default">$className </span><span class="keyword">= </span><span class="string">&apos;Bar2\Foo2&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo2 </span><span class="keyword">instanceof </span><span class="default">Bar2</span><span class="keyword">\</span><span class="default">Foo2</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo2 </span><span class="keyword">instanceof </span><span class="default">$className</span><span class="keyword">);<br></span><span class="default">$className </span><span class="keyword">= </span><span class="string">&apos;Foo2&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo2 </span><span class="keyword">instanceof </span><span class="default">$className</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>## stdout ##<br>bool(true)<br>bool(true)<br>bool(false)<br>bool(true)<br>bool(true)<br>bool(false)</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use &quot;self&quot; to reference to the current class:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">myclass </span><span class="keyword">{
<br>&#xA0; &#xA0; function </span><span class="default">mymethod</span><span class="keyword">(</span><span class="default">$otherObject</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$otherObject </span><span class="keyword">instanceof </span><span class="default">self</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$otherObject</span><span class="keyword">-&gt;</span><span class="default">mymethod</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;works!&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$a </span><span class="keyword">= new </span><span class="default">myclass</span><span class="keyword">();
<br>print </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">mymethod</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.type.php)

**[To root](/README.md)**