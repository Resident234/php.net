# get_class




<div class="phpcode"><span class="html">
&gt;= 5.5<br><br>::class<br>fully qualified class name, instead of get_class<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">my</span><span class="keyword">\</span><span class="default">library</span><span class="keyword">\</span><span class="default">mvc</span><span class="keyword">;<br><br>class </span><span class="default">Dispatcher </span><span class="keyword">{}<br><br>print </span><span class="default">Dispatcher</span><span class="keyword">::class; </span><span class="comment">// FQN == my\library\mvc\Dispatcher<br><br></span><span class="default">$disp </span><span class="keyword">= new </span><span class="default">Dispatcher</span><span class="keyword">;<br><br>print </span><span class="default">$disp</span><span class="keyword">::class; </span><span class="comment">// parse error</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A lot of people in other comments wanting to get the classname without the namespace. Some weird suggestions of code to do that - not what I would&apos;ve written! So wanted to add my own way.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">get_class_name</span><span class="keyword">(</span><span class="default">$classname</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if (</span><span class="default">$pos </span><span class="keyword">= </span><span class="default">strrpos</span><span class="keyword">(</span><span class="default">$classname</span><span class="keyword">, </span><span class="string">&apos;\\&apos;</span><span class="keyword">)) return </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$classname</span><span class="keyword">, </span><span class="default">$pos </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$pos</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Also did some quick benchmarking, and strrpos() was the fastest too. Micro-optimisations = macro optimisations!<br><br>39.0954 ms - preg_match()<br>28.6305 ms - explode() + end()<br>20.3314 ms - strrpos()<br><br>(For reference, here&apos;s the debug code used. c() is a benchmarking function that runs each closure run 10,000 times.)<br><br><span class="default">&lt;?php<br>c</span><span class="keyword">(<br>&#xA0; &#xA0; function(</span><span class="default">$class </span><span class="keyword">= </span><span class="string">&apos;a\b\C&apos;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\\([\w]+)$/&apos;</span><span class="keyword">, </span><span class="default">$class</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">)) return </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$class</span><span class="keyword">;<br>&#xA0; &#xA0; },<br>&#xA0; &#xA0; function(</span><span class="default">$class </span><span class="keyword">= </span><span class="string">&apos;a\b\C&apos;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bits </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;\\&apos;</span><span class="keyword">, </span><span class="default">$class</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">end</span><span class="keyword">(</span><span class="default">$bits</span><span class="keyword">);<br>&#xA0; &#xA0; },<br>&#xA0; &#xA0; function(</span><span class="default">$class </span><span class="keyword">= </span><span class="string">&apos;a\b\C&apos;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$pos </span><span class="keyword">= </span><span class="default">strrpos</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="string">&apos;\\&apos;</span><span class="keyword">)) return </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$pos </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$pos</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
People seem to mix up what __METHOD__, get_class($obj) and get_class() do, related to class inheritance.
<br>
<br>Here&apos;s a good example that should fix that for ever:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{
<br> function </span><span class="default">doMethod</span><span class="keyword">(){
<br>&#xA0; echo </span><span class="default">__METHOD__ </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br> }
<br> function </span><span class="default">doGetClassThis</span><span class="keyword">(){
<br>&#xA0; echo </span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">).</span><span class="string">&apos;::doThat&apos; </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br> }
<br> function </span><span class="default">doGetClass</span><span class="keyword">(){
<br>&#xA0; echo </span><span class="default">get_class</span><span class="keyword">().</span><span class="string">&apos;::doThat&apos; </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br> }
<br>}
<br>
<br>class </span><span class="default">Bar </span><span class="keyword">extends </span><span class="default">Foo </span><span class="keyword">{
<br>
<br>}
<br>
<br>class </span><span class="default">Quux </span><span class="keyword">extends </span><span class="default">Bar </span><span class="keyword">{
<br> function </span><span class="default">doMethod</span><span class="keyword">(){
<br>&#xA0; echo </span><span class="default">__METHOD__ </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br> }
<br> function </span><span class="default">doGetClassThis</span><span class="keyword">(){
<br>&#xA0; echo </span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">).</span><span class="string">&apos;::doThat&apos; </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br> }
<br> function </span><span class="default">doGetClass</span><span class="keyword">(){
<br>&#xA0; echo </span><span class="default">get_class</span><span class="keyword">().</span><span class="string">&apos;::doThat&apos; </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br> }
<br>}
<br>
<br></span><span class="default">$foo </span><span class="keyword">= new </span><span class="default">Foo</span><span class="keyword">();
<br></span><span class="default">$bar </span><span class="keyword">= new </span><span class="default">Bar</span><span class="keyword">();
<br></span><span class="default">$quux </span><span class="keyword">= new </span><span class="default">Quux</span><span class="keyword">();
<br>
<br>echo </span><span class="string">&quot;\n--doMethod--\n&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">doMethod</span><span class="keyword">();
<br></span><span class="default">$bar</span><span class="keyword">-&gt;</span><span class="default">doMethod</span><span class="keyword">();
<br></span><span class="default">$quux</span><span class="keyword">-&gt;</span><span class="default">doMethod</span><span class="keyword">();
<br>
<br>echo </span><span class="string">&quot;\n--doGetClassThis--\n&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">doGetClassThis</span><span class="keyword">();
<br></span><span class="default">$bar</span><span class="keyword">-&gt;</span><span class="default">doGetClassThis</span><span class="keyword">();
<br></span><span class="default">$quux</span><span class="keyword">-&gt;</span><span class="default">doGetClassThis</span><span class="keyword">();
<br>
<br>echo </span><span class="string">&quot;\n--doGetClass--\n&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">doGetClass</span><span class="keyword">();
<br></span><span class="default">$bar</span><span class="keyword">-&gt;</span><span class="default">doGetClass</span><span class="keyword">();
<br></span><span class="default">$quux</span><span class="keyword">-&gt;</span><span class="default">doGetClass</span><span class="keyword">();
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>OUTPUT:
<br>
<br>--doMethod--
<br>Foo::doMethod
<br>Foo::doMethod
<br>Quux::doMethod
<br>
<br>--doGetClassThis--
<br>Foo::doThat
<br>Bar::doThat
<br>Quux::doThat
<br>
<br>--doGetClass--
<br>Foo::doThat
<br>Foo::doThat
<br>Quux::doThat</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-class.php)

**[⬆ to root](/)**