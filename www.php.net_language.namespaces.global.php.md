# Global space




<div class="phpcode"><span class="html">
Included files will default to the global namespace.
<br><span class="default">&lt;?php
<br></span><span class="comment">//test.php
<br></span><span class="keyword">namespace </span><span class="default">test </span><span class="keyword">{
<br>&#xA0; include </span><span class="string">&apos;test1.inc&apos;</span><span class="keyword">;
<br>&#xA0; echo </span><span class="string">&apos;-&apos;</span><span class="keyword">,</span><span class="default">__NAMESPACE__</span><span class="keyword">,</span><span class="string">&apos;-&lt;br /&gt;&apos;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br><span class="default">&lt;?php
<br></span><span class="comment">//test1.inc
<br>&#xA0; </span><span class="keyword">echo </span><span class="string">&apos;-&apos;</span><span class="keyword">,</span><span class="default">__NAMESPACE__</span><span class="keyword">,</span><span class="string">&apos;-&lt;br /&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Results of test.php:
<br>
<br>--
<br>-test-</span>
</div>
  

#


<div class="phpcode"><span class="html">
In namespaced context the Exception class needs to be prefixed with global prefix operator.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">namespace </span><span class="default">hey</span><span class="keyword">\</span><span class="default">ho</span><span class="keyword">\</span><span class="default">lets</span><span class="keyword">\</span><span class="default">go</span><span class="keyword">;<br><br>class </span><span class="default">MyClass<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">failToCatch</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; try {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$thing </span><span class="keyword">= </span><span class="default">somethingThrowingAnException</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; } catch (</span><span class="default">Exception $ex</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Not catched<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">succeedToCatch</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; try {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$thing </span><span class="keyword">= </span><span class="default">somethingThrowingAnException</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; } catch (\</span><span class="default">Exception $ex</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// This is now catched<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; }<br><br>}</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.global.php)

**[To root](/)**