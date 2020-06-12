# The Closure class




<div class="phpcode"><span class="html">
This caused me some confusion a while back when I was still learning what closures were and how to use them, but what is referred to as a closure in PHP isn&apos;t the same thing as what they call closures in other languages (E.G. JavaScript).<br><br>In JavaScript, a closure can be thought of as a scope, when you define a function, it silently inherits the scope it&apos;s defined in, which is called its closure, and it retains that no matter where it&apos;s used.&#xA0; It&apos;s possible for multiple functions to share the same closure, and they can have access to multiple closures as long as they are within their accessible scope.<br><br>In PHP,&#xA0; a closure is a callable class, to which you&apos;ve bound your parameters manually.<br><br>It&apos;s a slight distinction but one I feel bears mentioning.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Small little trick. You can use a closures in itself via reference.<br><br>Example to delete a directory with all subdirectories and files:<br><br><span class="default">&lt;?php<br>$deleteDirectory </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br></span><span class="default">$deleteDirectory </span><span class="keyword">= function(</span><span class="default">$path</span><span class="keyword">) use (&amp;</span><span class="default">$deleteDirectory</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$resource </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">);<br>&#xA0; &#xA0; while ((</span><span class="default">$item </span><span class="keyword">= </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$resource</span><span class="keyword">)) !== </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$item </span><span class="keyword">!== </span><span class="string">&quot;.&quot; </span><span class="keyword">&amp;&amp; </span><span class="default">$item </span><span class="keyword">!== </span><span class="string">&quot;..&quot;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$path </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$item</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$deleteDirectory</span><span class="keyword">(</span><span class="default">$path </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$item</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$path </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$item</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$resource</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">);<br>};<br></span><span class="default">$deleteDirectory</span><span class="keyword">(</span><span class="string">&quot;path/to/directoy&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Scope<br>A closure encapsulates its scope, meaning that it has no access to the scope in which it is defined or executed. It is, however, possible to inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword:<br><br>function createGreeter($who) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return function() use ($who) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Hello $who&quot;;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; };<br>}<br><br>$greeter = createGreeter(&quot;World&quot;);<br>$greeter(); // Hello World<br><br>This inherits the variables by-value, that is, a copy is made available inside the closure using its original name.<br>font: Zend Certification Study Guide.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.closure.php)

**[To root](/)**