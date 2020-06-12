# is_callable




<div class="phpcode"><span class="html">
If the target class has __call() magic function implemented, then is_callable will ALWAYS return TRUE for whatever method you call it.<br>is_callable does not evaluate your internal logic inside __call() implementation (and this is for good).<br>Therefore every method name is callable for such classes.<br><br>Hence it is WRONG to say (as someone said):<br>...is_callable will correctly determine the existence of methods made with __call...<br><br>Example:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">TestCallable<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">testing</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&quot;I am called.&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">__call</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$name </span><span class="keyword">== </span><span class="string">&apos;testingOther&apos;</span><span class="keyword">) <br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;testing&apos;</span><span class="keyword">), </span><span class="default">$args</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$t </span><span class="keyword">= new </span><span class="default">TestCallable</span><span class="keyword">();<br>echo </span><span class="default">$t</span><span class="keyword">-&gt;</span><span class="default">testing</span><span class="keyword">();&#xA0; &#xA0; &#xA0; </span><span class="comment">// Output: I am called.<br></span><span class="keyword">echo </span><span class="default">$t</span><span class="keyword">-&gt;</span><span class="default">testingOther</span><span class="keyword">(); </span><span class="comment">// Output: I am called.<br></span><span class="keyword">echo </span><span class="default">$t</span><span class="keyword">-&gt;</span><span class="default">working</span><span class="keyword">();&#xA0; &#xA0; &#xA0; </span><span class="comment">// Output: (null)<br><br></span><span class="keyword">echo </span><span class="default">is_callable</span><span class="keyword">(array(</span><span class="default">$t</span><span class="keyword">, </span><span class="string">&apos;testing&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// Output: TRUE<br></span><span class="keyword">echo </span><span class="default">is_callable</span><span class="keyword">(array(</span><span class="default">$t</span><span class="keyword">, </span><span class="string">&apos;testingOther&apos;</span><span class="keyword">));&#xA0; </span><span class="comment">// Output: TRUE<br></span><span class="keyword">echo </span><span class="default">is_callable</span><span class="keyword">(array(</span><span class="default">$t</span><span class="keyword">, </span><span class="string">&apos;working&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// Output: TRUE, expected: FALSE<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
is_callable() will try __autoload(), if have one.</span>
</div>
  

#


<div class="phpcode"><span class="html">
is_callable doesn&apos;t seem able to resolve namespaces.&#xA0; If you&apos;re passing a string, then the string has to include the function&apos;s full namespace.&#xA0; <br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">\</span><span class="default">bar</span><span class="keyword">\</span><span class="default">baz</span><span class="keyword">;<br><br>function </span><span class="default">something </span><span class="keyword">()<br>{<br>&#xA0; &#xA0; return (</span><span class="default">42</span><span class="keyword">);<br>}<br><br></span><span class="default">var_dump </span><span class="keyword">(</span><span class="default">is_callable </span><span class="keyword">(</span><span class="string">&apos;something&apos;</span><span class="keyword">)); </span><span class="comment">// false<br></span><span class="default">var_dump </span><span class="keyword">(</span><span class="default">is_callable </span><span class="keyword">(</span><span class="string">&apos;foo\bar\baz\something&apos;</span><span class="keyword">)); </span><span class="comment">// true<br></span><span class="default">?&gt;<br></span><br>It&apos;s easy to forget, but if you just prepend __NAMESPACE__ to your function name strings you should be fine in most cases.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To corey at eyewantmedia dot com:<br><br>your misunderstanding lies in passing in the naked $object parameter. It is correct for is_callable to return FALSE since you cannot &apos;call an object&apos;, you can only call one of its methods, but you don&apos;t specify which one. Hence:<br><br>is_callable(array($object, &apos;some_function&apos;), [true or false], $callable_name)<br><br>will yield the correct result.<br><br>Notice, though, that a quick test I made (PHP 5.0.4) showed that is_callable incorrectly returns TRUE also if you specify the name of a protected/private method from outside of the context of the defining class, so, as wasti dot redl at gmx dot net pointed out, reflection is the way to go if you want to take visibility into account (which you should for true OOP, IMHO).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-callable.php)

**[⬆ to root](/)**