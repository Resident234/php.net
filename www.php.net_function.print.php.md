# print




<div class="phpcode"><span class="html">
Be careful when using print. Since print is a language construct and not a function, the parentheses around the argument is not required.<br>In fact, using parentheses can cause confusion with the syntax of a function and SHOULD be omited.<br><br>Most would expect the following behavior:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if (print(</span><span class="string">&quot;foo&quot;</span><span class="keyword">) &amp;&amp; print(</span><span class="string">&quot;bar&quot;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// &quot;foo&quot; and &quot;bar&quot; had been printed<br>&#xA0; &#xA0; </span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>But since the parenthesis around the argument are not required, they are interpretet as part of the argument.<br>This means that the argument of the first print is<br><br>&#xA0; &#xA0; (&quot;foo&quot;) &amp;&amp; print(&quot;bar&quot;)<br><br>and the argument of the second print is just<br><br>&#xA0; &#xA0; (&quot;bar&quot;)<br><br>For the expected behavior of the first example, you need to write: <br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if ((print </span><span class="string">&quot;foo&quot;</span><span class="keyword">) &amp;&amp; (print </span><span class="string">&quot;bar&quot;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// &quot;foo&quot; and &quot;bar&quot; had been printed<br>&#xA0; &#xA0; </span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I wrote a println function that determines whether a \n or a &lt;br /&gt; should be appended to the line depending on whether it&apos;s being executed in a shell or a browser window.&#xA0; People have probably thought of this before but I thought I&apos;d post it anyway - it may help a couple of people.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">println </span><span class="keyword">(</span><span class="default">$string_message</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;SERVER_PROTOCOL&apos;</span><span class="keyword">] ? print </span><span class="string">&quot;</span><span class="default">$string_message</span><span class="string">&lt;br /&gt;&quot; </span><span class="keyword">: print </span><span class="string">&quot;</span><span class="default">$string_message</span><span class="string">\n&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Examples:<br><br>Running in a browser:<br><br><span class="default">&lt;?php println </span><span class="keyword">(</span><span class="string">&quot;Hello, world!&quot;</span><span class="keyword">); </span><span class="default">?&gt;<br></span>Output: Hello, world!&lt;br /&gt;<br><br>Running in a shell:<br><br><span class="default">&lt;?php println </span><span class="keyword">(</span><span class="string">&quot;Hello, world!&quot;</span><span class="keyword">); </span><span class="default">?&gt;<br></span>Output: Hello, world!\n</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.print.php)

**[To root](/README.md)**