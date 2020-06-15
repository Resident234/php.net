# ReflectionProperty::setAccessible




<div class="phpcode"><span class="html">
Note that the property will only become accessible using the ReflectionProperty class. The property is still private or protected in the class instances.<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{<br>&#xA0; &#xA0;&#xA0; private </span><span class="default">$myProperty </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>}<br><br></span><span class="default">$class </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&quot;MyClass&quot;</span><span class="keyword">);<br></span><span class="default">$property </span><span class="keyword">= </span><span class="default">$class</span><span class="keyword">-&gt;</span><span class="default">getProperty</span><span class="keyword">(</span><span class="string">&quot;myProperty&quot;</span><span class="keyword">);<br></span><span class="default">$property</span><span class="keyword">-&gt;</span><span class="default">setAccessible</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br><br></span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">MyClass</span><span class="keyword">();<br>echo </span><span class="default">$property</span><span class="keyword">-&gt;</span><span class="default">getValue</span><span class="keyword">(</span><span class="default">$obj</span><span class="keyword">); </span><span class="comment">// Works<br></span><span class="keyword">echo </span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">myProperty</span><span class="keyword">; </span><span class="comment">// Doesn&apos;t work (error)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionproperty.setaccessible.php)

**[To root](/README.md)**