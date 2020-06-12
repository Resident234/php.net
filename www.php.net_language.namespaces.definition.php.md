# Defining namespaces




<div class="phpcode"><span class="html">
If your code looks like this:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">namespace </span><span class="default">NS</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>...and you still get &quot;Namespace declaration statement has to be the very first statement in the script&quot; Fatal error, then you probably use UTF-8 encoding (which is good) with Byte Order Mark, aka BOM (which is bad). Try to convert your files to &quot;UTF-8 without BOM&quot;, and it should be ok.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Regarding constants defined with define() inside namespaces...<br><br>define() will define constants exactly as specified.&#xA0; So, if you want to define a constant in a namespace, you will need to specify the namespace in your call to define(), even if you&apos;re calling define() from within a namespace.&#xA0; The following examples will make it clear.<br><br>The following code will define the constant &quot;MESSAGE&quot; in the global namespace (i.e. &quot;\MESSAGE&quot;).<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">test</span><span class="keyword">;<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;MESSAGE&apos;</span><span class="keyword">, </span><span class="string">&apos;Hello world!&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The following code will define two constants in the &quot;test&quot; namespace.<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">test</span><span class="keyword">;<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;test\HELLO&apos;</span><span class="keyword">, </span><span class="string">&apos;Hello world!&apos;</span><span class="keyword">);<br></span><span class="default">define</span><span class="keyword">(</span><span class="default">__NAMESPACE__ </span><span class="keyword">. </span><span class="string">&apos;\GOODBYE&apos;</span><span class="keyword">, </span><span class="string">&apos;Goodbye cruel world!&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Expanding on @danbettles note, it is better to always be explicit about which constant to use.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">namespace </span><span class="default">NS</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="default">define</span><span class="keyword">(</span><span class="default">__NAMESPACE__ </span><span class="keyword">.</span><span class="string">&apos;\foo&apos;</span><span class="keyword">,</span><span class="string">&apos;111&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;foo&apos;</span><span class="keyword">,</span><span class="string">&apos;222&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; echo </span><span class="default">foo</span><span class="keyword">;&#xA0; </span><span class="comment">// 111.<br>&#xA0; &#xA0; </span><span class="keyword">echo \</span><span class="default">foo</span><span class="keyword">;&#xA0; </span><span class="comment">// 222.<br>&#xA0; &#xA0; </span><span class="keyword">echo \</span><span class="default">NS</span><span class="keyword">\</span><span class="default">foo&#xA0; </span><span class="comment">// 111.<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">NS</span><span class="keyword">\</span><span class="default">foo&#xA0; </span><span class="comment">// fatal error. assumes \NS\NS\foo.<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;A file containing a namespace must declare the namespace at the top of the file before any other code&quot;
<br>
<br>It might be obvious, but this means that you *can* include comments and white spaces before the namespace keyword.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// Lots 
<br>// of
<br>// interesting
<br>// comments and white space
<br>
<br></span><span class="keyword">namespace </span><span class="default">Foo</span><span class="keyword">;
<br>class </span><span class="default">Bar </span><span class="keyword">{
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You should not try to create namespaces that use PHP keywords. These will cause parse errors. 
<br>
<br>Examples:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">namespace </span><span class="default">Project</span><span class="keyword">/</span><span class="default">Classes</span><span class="keyword">/Function; </span><span class="comment">// Causes parse errors
<br></span><span class="keyword">namespace </span><span class="default">Project</span><span class="keyword">/Abstract/</span><span class="default">Factory</span><span class="keyword">; </span><span class="comment">// Causes parse errors
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definition.php)

**[⬆ to root](/)**