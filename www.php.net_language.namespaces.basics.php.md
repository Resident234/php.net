# Using namespaces: Basics




<div class="phpcode"><span class="html">
Syntax for extending classes in namespaces is still the same.
<br>
<br>Lets call this Object.php:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">namespace </span><span class="default">com</span><span class="keyword">\</span><span class="default">rsumilang</span><span class="keyword">\</span><span class="default">common</span><span class="keyword">;
<br>
<br>class </span><span class="default">Object</span><span class="keyword">{
<br>&#xA0;&#xA0; </span><span class="comment">// ... code ...
<br></span><span class="keyword">}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>And now lets create a class called String that extends object in String.php:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">String </span><span class="keyword">extends </span><span class="default">com</span><span class="keyword">\</span><span class="default">rsumilang</span><span class="keyword">\</span><span class="default">common</span><span class="keyword">\</span><span class="default">Object</span><span class="keyword">{
<br>&#xA0;&#xA0; </span><span class="comment">// ... code ...
<br></span><span class="keyword">}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Now if you class String was defined in the same namespace as Object then you don&apos;t have to specify a full namespace path:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">namespace </span><span class="default">com</span><span class="keyword">\</span><span class="default">rsumilang</span><span class="keyword">\</span><span class="default">common</span><span class="keyword">;
<br>
<br>class </span><span class="default">String </span><span class="keyword">extends </span><span class="default">Object
<br></span><span class="keyword">{
<br>&#xA0;&#xA0; </span><span class="comment">// ... code ...
<br></span><span class="keyword">}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Lastly, you can also alias a namespace name to use a shorter name for the class you are extending incase your class is in seperate namespace:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">namespace </span><span class="default">com</span><span class="keyword">\</span><span class="default">rsumilang</span><span class="keyword">\</span><span class="default">util</span><span class="keyword">;
<br>use </span><span class="default">com</span><span class="keyword">\</span><span class="default">rsumlang</span><span class="keyword">\</span><span class="default">common </span><span class="keyword">as </span><span class="default">Common</span><span class="keyword">;
<br>
<br>class </span><span class="default">String </span><span class="keyword">extends </span><span class="default">Common</span><span class="keyword">\</span><span class="default">Object
<br></span><span class="keyword">{
<br>&#xA0;&#xA0; </span><span class="comment">// ... code ...
<br></span><span class="keyword">}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>- Richard Sumilang</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="keyword">namespace </span><span class="default">Foo</span><span class="keyword">;<br><br>try {<br>&#xA0; &#xA0; </span><span class="comment">// Something awful here<br>&#xA0; &#xA0; // That will throw a new exception from SPL<br></span><span class="keyword">} <br>catch (</span><span class="default">Exception </span><span class="keyword">as </span><span class="default">$ex</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// We will never get here<br>&#xA0; &#xA0; // This is because we are catchin Foo\Exception<br></span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>Instead use fully qualified name for the exception to catch it<br><br><span class="default">&lt;?php <br><br></span><span class="keyword">namespace </span><span class="default">Foo</span><span class="keyword">;<br><br>try {<br>&#xA0; &#xA0; </span><span class="comment">// something awful here<br>&#xA0; &#xA0; // That will throw a new exception from SPL<br></span><span class="keyword">} <br>catch (\</span><span class="default">Exception </span><span class="keyword">as </span><span class="default">$ex</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// Now we can get here at last<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Well variables inside namespaces do not override others since variables are never affected by namespace but always global:<br>&quot;Although any valid PHP code can be contained within a namespace, only four types of code are affected by namespaces: classes, interfaces, functions and constants. &quot;<br><br>Source: &quot;Defining Namespaces&quot;<br><a href="http://www.php.net/manual/en/language.namespaces.definition.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/language.namespaces.definition.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems the file system analogy only goes so far. One thing that&apos;s missing that would be very useful is relative navigation up the namespace chain, e.g.<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">MyProject </span><span class="keyword">{<br>&#xA0;&#xA0; class </span><span class="default">Person </span><span class="keyword">{}<br>}<br><br>namespace </span><span class="default">MyProject</span><span class="keyword">\</span><span class="default">People </span><span class="keyword">{<br>&#xA0; &#xA0; class </span><span class="default">Adult </span><span class="keyword">extends ..\</span><span class="default">Person </span><span class="keyword">{}<br>}<br></span><span class="default">?&gt;<br></span><br>That would be really nice, especially if you had really deep namespaces. It would save you having to type out the full namespace just to reference a resource one level up.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.basics.php)

**[⬆ to root](/)**