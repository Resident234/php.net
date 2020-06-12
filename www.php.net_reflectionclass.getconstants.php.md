# ReflectionClass::getConstants




<div class="phpcode"><span class="html">
If you want to return the constants defined inside a class then you can also define an internal method as follows:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">myClass </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">NONE </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; const </span><span class="default">REQUEST </span><span class="keyword">= </span><span class="default">100</span><span class="keyword">;<br>&#xA0; &#xA0; const </span><span class="default">AUTH </span><span class="keyword">= </span><span class="default">101</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment">// others...<br><br>&#xA0; &#xA0; </span><span class="keyword">static function </span><span class="default">getConstants</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$oClass </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="default">__CLASS__</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$oClass</span><span class="keyword">-&gt;</span><span class="default">getConstants</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can pass $this as class for the ReflectionClass. __CLASS__ won&apos;t help if you extend the original class, because it is a magic constant based on the file itself.<br><br><span class="default">&lt;?php <br><br></span><span class="keyword">class </span><span class="default">Example </span><span class="keyword">{<br>&#xA0; const </span><span class="default">TYPE_A </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; const </span><span class="default">TYPE_B </span><span class="keyword">= </span><span class="string">&apos;hello&apos;</span><span class="keyword">;<br><br>&#xA0; public function </span><span class="default">getConstants</span><span class="keyword">()<br>&#xA0; {<br>&#xA0; &#xA0; </span><span class="default">$reflectionClass </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$reflectionClass</span><span class="keyword">-&gt;</span><span class="default">getConstants</span><span class="keyword">();<br>&#xA0; }<br>}<br><br></span><span class="default">$example </span><span class="keyword">= new </span><span class="default">Example</span><span class="keyword">();<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$example</span><span class="keyword">-&gt;</span><span class="default">getConstants</span><span class="keyword">());<br><br></span><span class="comment">// Result:<br></span><span class="keyword">array ( </span><span class="default">size </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">)<br>&#xA0; </span><span class="string">&apos;TYPE_A&apos; </span><span class="keyword">=&gt; </span><span class="default">int 1<br>&#xA0; </span><span class="string">&apos;TYPE_B&apos; </span><span class="keyword">=&gt; (string) </span><span class="string">&apos;hello&apos;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getconstants.php)

**[⬆ to root](/)**