# define




<div class="phpcode"><span class="html">
Be aware that if &quot;Notice&quot;-level error reporting is turned off, then trying to use a constant as a variable will result in it being interpreted as a string, if it has not been defined.<br><br>I was working on a program which included a config file which contained:<br><br><span class="default">&lt;?php<br>define</span><span class="keyword">(</span><span class="string">&apos;ENABLE_UPLOADS&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Since I wanted to remove the ability for uploads, I changed the file to read:<br><br><span class="default">&lt;?php<br></span><span class="comment">//define(&apos;ENABLE_UPLOADS&apos;, true);<br></span><span class="default">?&gt;<br></span><br>However, to my surprise, the program was still allowing uploads. Digging deeper into the code, I discovered this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if ( </span><span class="default">ENABLE_UPLOADS </span><span class="keyword">):<br></span><span class="default">?&gt;<br></span><br>Since &apos;ENABLE_UPLOADS&apos; was not defined as a constant, PHP was interpreting its use as a string constant, which of course evaluates as True.</span>
</div>
  

#


<div class="phpcode"><span class="html">
define() will define constants exactly as specified.&#xA0; So, if you want to define a constant in a namespace, you will need to specify the namespace in your call to define(), even if you&apos;re calling define() from within a namespace.&#xA0; The following examples will make it clear.<br><br>The following code will define the constant &quot;MESSAGE&quot; in the global namespace (i.e. &quot;\MESSAGE&quot;).<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">test</span><span class="keyword">;<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;MESSAGE&apos;</span><span class="keyword">, </span><span class="string">&apos;Hello world!&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The following code will define two constants in the &quot;test&quot; namespace.<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">test</span><span class="keyword">;<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;test\HELLO&apos;</span><span class="keyword">, </span><span class="string">&apos;Hello world!&apos;</span><span class="keyword">);<br></span><span class="default">define</span><span class="keyword">(</span><span class="default">__NAMESPACE__ </span><span class="keyword">. </span><span class="string">&apos;\GOODBYE&apos;</span><span class="keyword">, </span><span class="string">&apos;Goodbye cruel world!&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.define.php)

**[To root](/README.md)**