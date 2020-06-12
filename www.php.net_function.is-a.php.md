# is_a




<div class="phpcode"><span class="html">
Please note that you have to fully qualify the class name in the second parameter. <br><br>A use statement will not resolve namespace dependencies in that is_a() function. <br><br><span class="default">&lt;?php <br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">\</span><span class="default">bar</span><span class="keyword">;<br><br>class </span><span class="default">A </span><span class="keyword">{};<br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A </span><span class="keyword">{};<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">har</span><span class="keyword">\var;<br><br>use </span><span class="default">foo</span><span class="keyword">\</span><span class="default">bar</span><span class="keyword">\</span><span class="default">A</span><span class="keyword">;<br></span><span class="default">$foo </span><span class="keyword">= new </span><span class="default">foo</span><span class="keyword">\</span><span class="default">bar</span><span class="keyword">\</span><span class="default">B</span><span class="keyword">();<br><br></span><span class="default">is_a</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">, </span><span class="string">&apos;A&apos;</span><span class="keyword">); </span><span class="comment">// returns false;<br></span><span class="default">is_a</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">, </span><span class="string">&apos;foo\bar\A&apos;</span><span class="keyword">); </span><span class="comment">// returns true;<br></span><span class="default">?&gt;<br></span><br>Just adding that note here because all examples are without namespaces.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful! Starting in PHP 5.3.7 the behavior of is_a() has changed slightly: when calling is_a() with a first argument that is not an object, __autoload() is triggered!<br><br>In practice, this means that calling is_a(&apos;23&apos;, &apos;User&apos;); will trigger __autoload() on &quot;23&quot;. Previously, the above statement simply returned &apos;false&apos;.<br><br>More info can be found here:<br><a href="https://bugs.php.net/bug.php?id=55475" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=55475</a><br><br>Whether this change is considered a bug and whether it will be reverted or kept in future versions is yet to be determined, but nevertheless it is how it is, for now...</span>
</div>
  

#


<div class="phpcode"><span class="html">
At least in PHP 5.1.6 this works as well with Interfaces.<br><br><span class="default">&lt;?php<br></span><span class="keyword">interface </span><span class="default">test </span><span class="keyword">{<br>&#xA0; public function </span><span class="default">A</span><span class="keyword">();<br>}<br><br>class </span><span class="default">TestImplementor </span><span class="keyword">implements </span><span class="default">test </span><span class="keyword">{<br>&#xA0; public function </span><span class="default">A </span><span class="keyword">() {<br>&#xA0; &#xA0; print </span><span class="string">&quot;A&quot;</span><span class="keyword">;<br>&#xA0; }<br>}<br><br></span><span class="default">$testImpl </span><span class="keyword">= new </span><span class="default">TestImplementor</span><span class="keyword">();<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">is_a</span><span class="keyword">(</span><span class="default">$testImpl</span><span class="keyword">,</span><span class="string">&apos;test&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>will return true</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-a.php)

**[⬆ to root](/)**