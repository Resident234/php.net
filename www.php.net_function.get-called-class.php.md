# get_called_class




<div class="phpcode"><span class="html">
As of PHP 5.5 you can also use &quot;static::class&quot; to get the name of the called class.<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Bar </span><span class="keyword">{<br>&#xA0; &#xA0; public static function </span><span class="default">test</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(static::class);<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">Foo </span><span class="keyword">extends </span><span class="default">Bar </span><span class="keyword">{<br><br>}<br><br></span><span class="default">Foo</span><span class="keyword">::</span><span class="default">test</span><span class="keyword">();<br></span><span class="default">Bar</span><span class="keyword">::</span><span class="default">test</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>Output:<br><br>string(3) &quot;Foo&quot;<br>string(3) &quot;Bar&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
SEE: <a href="http://php.net/manual/en/language.oop5.late-static-bindings.php" rel="nofollow" target="_blank">http://php.net/manual/en/language.oop5.late-static-bindings.php</a><br><br>I think it is worth mentioning on this page, that many uses of the value returned by get_called_function() could be handled with the new use of the old keyword static, as in<br><span class="default">&lt;?php <br></span><span class="keyword">static::</span><span class="default">$foo</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>versus<br><span class="default">&lt;?php<br>$that</span><span class="keyword">=</span><span class="default">get_called_class</span><span class="keyword">();<br></span><span class="default">$that</span><span class="keyword">::</span><span class="default">$foo</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>I had been using $that:: as my conventional replacement for self:: until my googling landed me the url above.&#xA0; I have replaced all uses of $that with static with success both as <br><span class="default">&lt;?php <br></span><span class="keyword">static::</span><span class="default">$foo</span><span class="keyword">; </span><span class="comment">//and...<br></span><span class="keyword">new static();<br></span><span class="default">?&gt;<br></span><br>Since static:: is listed with the limitation: &quot;Another difference is that static:: can only refer to static properties.&quot; one may still need to use a $that:: to call static functions; though I have not yet needed this semantic.</span>
</div>
  

#


<div class="phpcode"><span class="html">
get_called_class() in closure-scopes:<br><br><span class="default">&lt;?PHP<br>&#xA0; &#xA0; </span><span class="keyword">ABSTRACT CLASS </span><span class="default">Base<br>&#xA0; &#xA0; </span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; protected static </span><span class="default">$stub </span><span class="keyword">= [</span><span class="string">&apos;baz&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//final public function boot()<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">static public function </span><span class="default">boot</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">__METHOD__</span><span class="keyword">.</span><span class="string">&apos;-&gt; &apos;</span><span class="keyword">.</span><span class="default">get_called_class</span><span class="keyword">().</span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_walk</span><span class="keyword">(static::</span><span class="default">$stub</span><span class="keyword">, function()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">__METHOD__</span><span class="keyword">.</span><span class="string">&apos;-&gt; &apos;</span><span class="keyword">.</span><span class="default">get_called_class</span><span class="keyword">().</span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; });<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">boot</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">__METHOD__</span><span class="keyword">.</span><span class="string">&apos;-&gt; &apos;</span><span class="keyword">.</span><span class="default">get_called_class</span><span class="keyword">().</span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_walk</span><span class="keyword">(static::</span><span class="default">$stub</span><span class="keyword">, function()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">__METHOD__</span><span class="keyword">.</span><span class="string">&apos;-&gt; &apos;</span><span class="keyword">.</span><span class="default">get_called_class</span><span class="keyword">().</span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; });<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; CLASS </span><span class="default">Sub </span><span class="keyword">EXTENDS </span><span class="default">Base<br>&#xA0; &#xA0; </span><span class="keyword">{<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// static boot<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">Base</span><span class="keyword">::</span><span class="default">boot</span><span class="keyword">(); print </span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Base::boot&#xA0; &#xA0; &#xA0; &#xA0; -&gt; Base<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base::{closure}&#xA0; &#xA0; -&gt; Base<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">Sub</span><span class="keyword">::</span><span class="default">boot</span><span class="keyword">(); print </span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Base::boot&#xA0; &#xA0; &#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base::{closure}&#xA0; &#xA0; -&gt; Base<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">new </span><span class="default">sub</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Base::boot&#xA0; &#xA0; &#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base::{closure}&#xA0; &#xA0; -&gt; Base<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base-&gt;__construct&#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base-&gt;{closure}&#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; // instance boot<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">new </span><span class="default">sub</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Base-&gt;boot&#xA0; &#xA0; &#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base-&gt;{closure}&#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base-&gt;__construct&#xA0; &#xA0; -&gt; Sub<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Base-&gt;{closure}&#xA0; &#xA0; -&gt; Sub<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is possible to write a completely self-contained Singleton base class in PHP 5.3 using get_called_class.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">abstract class </span><span class="default">Singleton </span><span class="keyword">{
<br>
<br>&#xA0; &#xA0; protected function </span><span class="default">__construct</span><span class="keyword">() {
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; final public static function </span><span class="default">getInstance</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; static </span><span class="default">$aoInstance </span><span class="keyword">= array();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$calledClassName </span><span class="keyword">= </span><span class="default">get_called_class</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (! isset (</span><span class="default">$aoInstance</span><span class="keyword">[</span><span class="default">$calledClassName</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$aoInstance</span><span class="keyword">[</span><span class="default">$calledClassName</span><span class="keyword">] = new </span><span class="default">$calledClassName</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$aoInstance</span><span class="keyword">[</span><span class="default">$calledClassName</span><span class="keyword">];
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; final private function </span><span class="default">__clone</span><span class="keyword">() {
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>class </span><span class="default">DatabaseConnection </span><span class="keyword">extends </span><span class="default">Singleton </span><span class="keyword">{
<br>
<br>&#xA0; &#xA0; protected </span><span class="default">$connection</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; protected function </span><span class="default">__construct</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// @todo Connect to the database
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">__destruct</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// @todo Drop the connection to the database
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>}
<br>
<br></span><span class="default">$oDbConn </span><span class="keyword">= new </span><span class="default">DatabaseConnection</span><span class="keyword">();&#xA0; </span><span class="comment">// Fatal error
<br>
<br></span><span class="default">$oDbConn </span><span class="keyword">= </span><span class="default">DatabaseConnection</span><span class="keyword">::</span><span class="default">getInstance</span><span class="keyword">();&#xA0; </span><span class="comment">// Returns single instance
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-called-class.php)

**[⬆ to root](/)**