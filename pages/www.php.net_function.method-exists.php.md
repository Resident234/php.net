# method_exists




<div class="phpcode"><span class="html">
As noted [elsewhere] method_exists() does not care about the existence of __call(), whereas is_callable() does:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">Test </span><span class="keyword">{
<br>&#xA0; public function </span><span class="default">explicit</span><span class="keyword">(&#xA0; ) {
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// ...
<br>&#xA0; </span><span class="keyword">}
<br>&#xA0;&#xA0; 
<br>&#xA0; public function </span><span class="default">__call</span><span class="keyword">( </span><span class="default">$meth</span><span class="keyword">, </span><span class="default">$args </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// ...
<br>&#xA0; </span><span class="keyword">}
<br>}
<br>
<br></span><span class="default">$Tester </span><span class="keyword">= new </span><span class="default">Test</span><span class="keyword">();
<br>
<br></span><span class="default">var_export</span><span class="keyword">(</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$Tester</span><span class="keyword">, </span><span class="string">&apos;anything&apos;</span><span class="keyword">)); </span><span class="comment">// false
<br></span><span class="default">var_export</span><span class="keyword">(</span><span class="default">is_callable</span><span class="keyword">(array(</span><span class="default">$Tester</span><span class="keyword">, </span><span class="string">&apos;anything&apos;</span><span class="keyword">))); </span><span class="comment">// true
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A couple of the older comments (specifically &quot;jp at function dot fi&quot; and &quot;spam at majiclab dot com&quot;) state that is_callable() does not respect a methods visibility when checked outside of that class. ie. That private/protected methods are seen as callable when tested publicly. However, this was a bug (#29210) in early versions of PHP 5 and was fixed (according to the changelog) in PHP 5.0.5 (and/or PHP 5.1.0).<br><br>Bug #29210 - Function: is_callable - no support for private and protected classes<br><a href="http://bugs.php.net/29210" rel="nofollow" target="_blank">http://bugs.php.net/29210</a><br><br>Changelog - Fixed bug #29210 (Function: is_callable - no support for private and protected classes). (Dmitry)<br><a href="http://php.net/ChangeLog-5.php#5.1.0" rel="nofollow" target="_blank">http://php.net/ChangeLog-5.php#5.1.0</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
This function is case-insensitive (as is PHP) and here is the proof:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">FUNC</span><span class="keyword">() { echo </span><span class="string">&apos;*****&apos;</span><span class="keyword">; }<br>}<br><br></span><span class="default">$a </span><span class="keyword">= new </span><span class="default">A</span><span class="keyword">();<br></span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">func</span><span class="keyword">(); </span><span class="comment">// *****<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="string">&apos;func&apos;</span><span class="keyword">)); </span><span class="comment">// bool(true)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just to mention it: both method_exists() and is_callable() return true for inherited methods:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">ParentClass </span><span class="keyword">{<br><br>&#xA0;&#xA0; function </span><span class="default">doParent</span><span class="keyword">() { }<br>}<br><br>class </span><span class="default">ChildClass </span><span class="keyword">extends </span><span class="default">ParentClass </span><span class="keyword">{ }<br><br></span><span class="default">$p </span><span class="keyword">= new </span><span class="default">ParentClass</span><span class="keyword">();<br></span><span class="default">$c </span><span class="keyword">= new </span><span class="default">ChildClass</span><span class="keyword">();<br><br></span><span class="comment">// all return true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$p</span><span class="keyword">, </span><span class="string">&apos;doParent&apos;</span><span class="keyword">));<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">, </span><span class="string">&apos;doParent&apos;</span><span class="keyword">));<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">is_callable</span><span class="keyword">(array(</span><span class="default">$p</span><span class="keyword">, </span><span class="string">&apos;doParent&apos;</span><span class="keyword">)));<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">is_callable</span><span class="keyword">(array(</span><span class="default">$c</span><span class="keyword">, </span><span class="string">&apos;doParent&apos;</span><span class="keyword">)));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you want to check for a method &quot;inside&quot; of a class use:<br><br>method_exists($this, &apos;function_name&apos;)<br><br>i was a bit confused &apos;cause i thought i&apos;m only able to check for a method when i got an object like $object_name = new class_name() with: <br><br>method_exists($object_name, &apos;method_name&apos;)<br><br>small example for those who didn&apos;t understood what i mean ( maybe caused by bad english :) ):<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">a </span><span class="keyword">{<br><br>&#xA0; &#xA0; function </span><span class="default">a</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;test&apos;</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;a::test() exists!&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;a::test() doesn\&apos;t exists&apos;</span><span class="keyword">;<br><br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; <br>&#xA0; &#xA0; function </span><span class="default">test</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">a</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>the output will be: a::test() exists!<br><br>maybe this will help someone</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.method-exists.php)

**[To root](/README.md)**