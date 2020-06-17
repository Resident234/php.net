# debug_backtrace




<div class="phpcode"><span class="html">
Here&apos;s a function I just wrote for getting a nice and comprehensible call trace. It is probably more resource-intensive than some other alternatives but it is short, understandable, and gives nice output (Exception-&gt;getTraceAsString()).<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">generateCallTrace</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; </span><span class="default">$e </span><span class="keyword">= new </span><span class="default">Exception</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$trace </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getTraceAsString</span><span class="keyword">());<br>&#xA0; &#xA0; </span><span class="comment">// reverse array to make steps line up chronologically<br>&#xA0; &#xA0; </span><span class="default">$trace </span><span class="keyword">= </span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">$trace</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$trace</span><span class="keyword">); </span><span class="comment">// remove {main}<br>&#xA0; &#xA0; </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$trace</span><span class="keyword">); </span><span class="comment">// remove call to this method<br>&#xA0; &#xA0; </span><span class="default">$length </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$trace</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= array();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$length</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[] = (</span><span class="default">$i </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">)&#xA0; . </span><span class="string">&apos;)&apos; </span><span class="keyword">. </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$trace</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$trace</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="string">&apos; &apos;</span><span class="keyword">)); </span><span class="comment">// replace &apos;#someNum&apos; with &apos;$i)&apos;, set the right ordering<br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="string">&quot;\t&quot; </span><span class="keyword">. </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;\n\t&quot;</span><span class="keyword">, </span><span class="default">$result</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Example output:<br>&#xA0; &#xA0; 1) /var/www/test/test.php(15): SomeClass-&gt;__construct()<br>&#xA0; &#xA0; 2) /var/www/test/SomeClass.class.php(36): SomeClass-&gt;callSomething()</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are using the backtrace function in an error handler, avoid using var_export() on the args, as you will cause fatal errors in some situations, preventing you from seeing your stack trace.&#xA0; Some structures will cause PHP to generate the fatal error &quot;Nesting level too deep - recursive dependency?&quot; This is a design feature of php, not a bug (see <a href="http://bugs.php.net/bug.php?id=30471" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=30471</a>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.debug-backtrace.php)

**[To root](/README.md)**