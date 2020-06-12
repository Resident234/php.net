# Namespaces and dynamic language features




<div class="phpcode"><span class="html">
When extending a class from another namespace that should instantiate a class from within the current namespace, you need to pass on the namespace.<br><br><span class="default">&lt;?php </span><span class="comment">// File1.php<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">;<br>class </span><span class="default">A </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">factory</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return new </span><span class="default">C</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br>class </span><span class="default">C </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">tell</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;foo&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php </span><span class="comment">// File2.php<br></span><span class="keyword">namespace </span><span class="default">bar</span><span class="keyword">;<br>class </span><span class="default">B </span><span class="keyword">extends \</span><span class="default">foo</span><span class="keyword">\</span><span class="default">A </span><span class="keyword">{}<br>class </span><span class="default">C </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">tell</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;bar&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="keyword">include </span><span class="string">&quot;File1.php&quot;</span><span class="keyword">;<br>include </span><span class="string">&quot;File2.php&quot;</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">bar</span><span class="keyword">\</span><span class="default">B</span><span class="keyword">;<br></span><span class="default">$c </span><span class="keyword">= </span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">factory</span><span class="keyword">();<br></span><span class="default">$c</span><span class="keyword">-&gt;</span><span class="default">tell</span><span class="keyword">(); </span><span class="comment">// &quot;foo&quot; but you want &quot;bar&quot;<br></span><span class="default">?&gt;<br></span><br>You need to do it like this:<br><br>When extending a class from another namespace that should instantiate a class from within the current namespace, you need to pass on the namespace.<br><br><span class="default">&lt;?php </span><span class="comment">// File1.php<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">;<br>class </span><span class="default">A </span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$namespace </span><span class="keyword">= </span><span class="default">__NAMESPACE__</span><span class="keyword">;<br>&#xA0; &#xA0; public function </span><span class="default">factory</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">namespace </span><span class="keyword">. </span><span class="string">&apos;\C&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return new </span><span class="default">$c</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br>class </span><span class="default">C </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">tell</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;foo&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php </span><span class="comment">// File2.php<br></span><span class="keyword">namespace </span><span class="default">bar</span><span class="keyword">;<br>class </span><span class="default">B </span><span class="keyword">extends \</span><span class="default">foo</span><span class="keyword">\</span><span class="default">A </span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$namespace </span><span class="keyword">= </span><span class="default">__NAMESPACE__</span><span class="keyword">;<br>}<br>class </span><span class="default">C </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">tell</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;bar&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="keyword">include </span><span class="string">&quot;File1.php&quot;</span><span class="keyword">;<br>include </span><span class="string">&quot;File2.php&quot;</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">bar</span><span class="keyword">\</span><span class="default">B</span><span class="keyword">;<br></span><span class="default">$c </span><span class="keyword">= </span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">factory</span><span class="keyword">();<br></span><span class="default">$c</span><span class="keyword">-&gt;</span><span class="default">tell</span><span class="keyword">(); </span><span class="comment">// &quot;bar&quot;<br></span><span class="default">?&gt;<br></span><br>(it seems that the namespace-backslashes are stripped from the source code in the preview, maybe it works in the main view. If not: fooA was written as \foo\A and barB as bar\B)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please be aware of FQCN (Full Qualified Class Name) point.
<br>Many people will have troubles with this:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">// File1.php
<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">;
<br>
<br>class </span><span class="default">Bar </span><span class="keyword">{ ... }
<br>
<br>function </span><span class="default">factory</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">) {
<br>&#xA0; &#xA0; return new </span><span class="default">$class</span><span class="keyword">;
<br>}
<br>
<br></span><span class="comment">// File2.php
<br></span><span class="default">$bar </span><span class="keyword">= \</span><span class="default">foo</span><span class="keyword">\</span><span class="default">factory</span><span class="keyword">(</span><span class="string">&apos;Bar&apos;</span><span class="keyword">); </span><span class="comment">// Will try to instantiate \Bar, not \foo\Bar
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>To fix that, and also incorporate a 2 step namespace resolution, you can check for \ as first char of $class, and if not present, build manually the FQCN:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">// File1.php
<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">;
<br>
<br>function </span><span class="default">factory</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">) {
<br>&#xA0; &#xA0; if (</span><span class="default">$class</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] != </span><span class="string">&apos;\\&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;-&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$class </span><span class="keyword">= </span><span class="string">&apos;\\&apos; </span><span class="keyword">. </span><span class="default">__NAMESPACE__ </span><span class="keyword">. </span><span class="string">&apos;\\&apos; </span><span class="keyword">. </span><span class="default">$class</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; return new </span><span class="default">$class</span><span class="keyword">();
<br>}
<br>
<br></span><span class="comment">// File2.php
<br></span><span class="default">$bar </span><span class="keyword">= \</span><span class="default">foo</span><span class="keyword">\</span><span class="default">factory</span><span class="keyword">(</span><span class="string">&apos;Bar&apos;</span><span class="keyword">); </span><span class="comment">// Will correctly instantiate \foo\Bar
<br>
<br></span><span class="default">$bar2 </span><span class="keyword">= \</span><span class="default">foo</span><span class="keyword">\</span><span class="default">factory</span><span class="keyword">(</span><span class="string">&apos;\anotherfoo\Bar&apos;</span><span class="keyword">); </span><span class="comment">// Wil correctly instantiate \anotherfoo\Bar
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.dynamic.php)

**[⬆ to root](/)**