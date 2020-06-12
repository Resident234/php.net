# empty




<div class="phpcode"><span class="html">
When you need to accept these as valid, non-empty values:<br>- 0 (0 as an integer)<br>- 0.0 (0 as a float)<br>- &quot;0&quot; (0 as a string)<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_blank</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; return empty(</span><span class="default">$value</span><span class="keyword">) &amp;&amp; !</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>This is similar to Rails&apos; blank? method.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that results of empty() when called on non-existing / non-public variables of a class are a bit confusing if using magic method __get (as previously mentioned by nahpeps at gmx dot de). Consider this example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Registry<br></span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$_items </span><span class="keyword">= array();<br>&#xA0; &#xA0; public function </span><span class="default">__set</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">__get</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$registry </span><span class="keyword">= new </span><span class="default">Registry</span><span class="keyword">();<br></span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">empty </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br></span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notEmpty </span><span class="keyword">= </span><span class="string">&apos;not empty&apos;</span><span class="keyword">;<br><br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notExisting</span><span class="keyword">)); </span><span class="comment">// true, so far so good<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">empty</span><span class="keyword">)); </span><span class="comment">// true, so far so good<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notEmpty</span><span class="keyword">)); </span><span class="comment">// true, .. say what?<br></span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notEmpty</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$tmp</span><span class="keyword">)); </span><span class="comment">// false as expected<br></span><span class="default">?&gt;<br></span><br>The result for empty($registry-&gt;notEmpty) is a bit unexpeced as the value is obviously set and non-empty. This is due to the fact that the empty() function uses __isset() magic functin in these cases. Although it&apos;s noted in the documentation above, I think it&apos;s worth mentioning in more detail as the behaviour is not straightforward. In order to achieve desired (expexted?) results, you need to add&#xA0; __isset() magic function to your class:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Registry<br></span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$_items </span><span class="keyword">= array();<br>&#xA0; &#xA0; public function </span><span class="default">__set</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">__get</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">__isset</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return (</span><span class="default">false </span><span class="keyword">=== empty(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_items</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]));<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$registry </span><span class="keyword">= new </span><span class="default">Registry</span><span class="keyword">();<br></span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">empty </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br></span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notEmpty </span><span class="keyword">= </span><span class="string">&apos;not empty&apos;</span><span class="keyword">;<br><br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notExisting</span><span class="keyword">)); </span><span class="comment">// true, so far so good<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">empty</span><span class="keyword">)); </span><span class="comment">// true, so far so good<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$registry</span><span class="keyword">-&gt;</span><span class="default">notEmpty</span><span class="keyword">)); </span><span class="comment">// false, finally!<br></span><span class="default">?&gt;<br></span><br>It actually seems that empty() is returning negation of the __isset() magic function result, hence the negation of the empty() result in the __isset() function above.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;m summarising a few points on empty() with inaccessible properties, in the hope of saving others a bit of time. Using PHP 5.3.2.<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$foo </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br>}<br></span><span class="default">$myClass </span><span class="keyword">= new </span><span class="default">MyClass</span><span class="keyword">;<br>echo </span><span class="default">$myClass</span><span class="keyword">-&gt;</span><span class="default">foo</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>As expected, this gives &quot;Fatal error: Cannot access private property MyClass::$foo&quot;.<br>But substitute the line<br>if (empty($myClass-&gt;foo)) echo &apos;foo is empty&apos;; else echo &apos;foo is not empty&apos;;<br>and we get the misleading result &quot;foo is empty&quot;. <br>There is NO ERROR OR WARNING, so this is a real gotcha. Your code will just go wrong silently, and I would say it amounts to a bug.<br>If you add two magic functions to the class:<br>public function __get($var) { return $this-&gt;$var; }<br>public function __isset($var) { return isset($this-&gt;$var); }<br>then we get the expected result. You need both functions.<br>For empty($myClass-&gt;foo), I believe PHP calls __isset, and if that is true returns the result of empty on the result of __get. (Some earlier posts wrongly suggest PHP just returns the negation of __isset).<br>BUT &#x2026;<br>See the earlier post by php at lanar dot com. I confirm those results, and if you extend the test with isset($x-&gt;a-&gt;b-&gt;c) it appears that __isset is only called for the last property in the chain. Arguably another bug. empty() behaves in the same way. So things are not as clear as we might hope.<br>See also the note on empty() at<br><a href="http://uk3.php.net/manual/en/language.oop5.overloading.php" rel="nofollow" target="_blank">http://uk3.php.net/manual/en/language.oop5.overloading.php</a><br>Clear as mud!</span>
</div>
  

#


<div class="phpcode"><span class="html">
To add on to what anon said, what&apos;s happening in john_jian&apos;s example seems unusual because we don&apos;t see the implicit typecasting going on behind the scenes.&#xA0; What&apos;s really happening is:<br><br>$a = &apos;&apos;;<br>$b = 0;<br>$c = &apos;0&apos;;<br><br>(int)$a == $b -&gt; true, because any string that&apos;s not a number gets converted to 0<br>$b==(int)$c -&gt; true, because the int in the string gets converted<br>and<br>$a==$c -&gt; false, because they&apos;re being compared as strings, rather than integers.&#xA0; (int)$a==(int)$c should return true, however.<br><br>Note: I don&apos;t remember if PHP even *has* typecasting, much less if this is the correct syntax.&#xA0; I&apos;m just using something for the sake of examples.</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br>$str </span><span class="keyword">= </span><span class="string">&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$str</span><span class="keyword">)); </span><span class="comment">// boolean false<br></span><span class="default">?&gt;<br></span><br>So remember to trim your strings first!<br><br><span class="default">&lt;?php<br>$str </span><span class="keyword">= </span><span class="string">&apos;&#xA0; &#xA0; &#xA0; &#xA0; &apos;</span><span class="keyword">;<br></span><span class="default">$str </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(empty(</span><span class="default">$str</span><span class="keyword">)); </span><span class="comment">// boolean true<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
test if all multiarray&apos;s are empty
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">is_multiArrayEmpty</span><span class="keyword">(</span><span class="default">$multiarray</span><span class="keyword">) {
<br>&#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$multiarray</span><span class="keyword">) and !empty(</span><span class="default">$multiarray</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$multiarray</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">is_multiArrayEmpty</span><span class="keyword">(</span><span class="default">$multiarray</span><span class="keyword">) or !</span><span class="default">is_multiArrayEmpty</span><span class="keyword">(</span><span class="default">$tmp</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(empty(</span><span class="default">$multiarray</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">$testCase </span><span class="keyword">= array (&#xA0; &#xA0;&#xA0; 
<br></span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&apos;&apos;</span><span class="keyword">,
<br></span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&quot;&quot;</span><span class="keyword">,
<br></span><span class="default">2 </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">,
<br></span><span class="default">3 </span><span class="keyword">=&gt; array(),
<br></span><span class="default">4 </span><span class="keyword">=&gt; array(array()),
<br></span><span class="default">5 </span><span class="keyword">=&gt; array(array(array(array(array())))),
<br></span><span class="default">6 </span><span class="keyword">=&gt; array(array(), array(), array(), array(), array()),
<br></span><span class="default">7 </span><span class="keyword">=&gt; array(array(array(), array()), array(array(array(array(array(array(), array())))))),
<br></span><span class="default">8 </span><span class="keyword">=&gt; array(</span><span class="default">null</span><span class="keyword">),
<br></span><span class="default">9 </span><span class="keyword">=&gt; </span><span class="string">&apos;not empty&apos;</span><span class="keyword">,
<br></span><span class="default">10 </span><span class="keyword">=&gt; </span><span class="string">&quot;not empty&quot;</span><span class="keyword">,
<br></span><span class="default">11 </span><span class="keyword">=&gt; array(array(</span><span class="string">&quot;not empty&quot;</span><span class="keyword">)),
<br></span><span class="default">12 </span><span class="keyword">=&gt; array(array(),array(</span><span class="string">&quot;not empty&quot;</span><span class="keyword">),array(array()))
<br>);
<br>
<br>foreach (</span><span class="default">$testCase </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$case </span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$key</span><span class="string"> is_multiArrayEmpty= &quot;</span><span class="keyword">.</span><span class="default">is_multiArrayEmpty</span><span class="keyword">(</span><span class="default">$case</span><span class="keyword">).</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>OUTPUT:
<br>========
<br>
<br>0 is_multiArrayEmpty= 1
<br>1 is_multiArrayEmpty= 1
<br>2 is_multiArrayEmpty= 1
<br>3 is_multiArrayEmpty= 1
<br>4 is_multiArrayEmpty= 1
<br>5 is_multiArrayEmpty= 1
<br>6 is_multiArrayEmpty= 1
<br>7 is_multiArrayEmpty= 1
<br>8 is_multiArrayEmpty= 1
<br>9 is_multiArrayEmpty= 
<br>10 is_multiArrayEmpty= 
<br>11 is_multiArrayEmpty= 
<br>12 is_multiArrayEmpty=</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.empty.php)

**[⬆ to root](/)**