# class_alias




<div class="phpcode"><span class="html">
class_alias() gives you the ability to do conditional imports.<br><br>Whereas the following will not work:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">Component</span><span class="keyword">;<br><br>if (</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">PHP_VERSION</span><span class="keyword">, </span><span class="string">&apos;5.4.0&apos;</span><span class="keyword">, </span><span class="string">&apos;gte&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; use </span><span class="default">My</span><span class="keyword">\</span><span class="default">ArrayObject</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; use </span><span class="default">ArrayObject</span><span class="keyword">;<br>}<br><br>class </span><span class="default">Container </span><span class="keyword">extends </span><span class="default">ArrayObject<br></span><span class="keyword">{<br>}<br></span><span class="default">?&gt;<br></span><br>the following, using class_alias, will:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">Component</span><span class="keyword">;<br><br>if (</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">PHP_VERSION</span><span class="keyword">, </span><span class="string">&apos;5.4.0&apos;</span><span class="keyword">, </span><span class="string">&apos;lt&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;My\ArrayObject&apos;</span><span class="keyword">, </span><span class="string">&apos;Component\ArrayObject&apos;</span><span class="keyword">);<br>} else {<br>&#xA0; &#xA0; </span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;ArrayObject&apos;</span><span class="keyword">, </span><span class="string">&apos;Component\ArrayObject&apos;</span><span class="keyword">);<br>}<br><br>class </span><span class="default">Container </span><span class="keyword">extends </span><span class="default">ArrayObject<br></span><span class="keyword">{<br>}<br></span><span class="default">?&gt;<br></span><br>The semantics are slightly different (I&apos;m now indicating that Container extends from an ArrayObject implementation in the same namespace), but the overall idea is the same: conditional imports.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you defined the class &apos;original&apos; in a namespace, you will have to specify the namespace(s), too:<br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">ns1</span><span class="keyword">\</span><span class="default">ns2</span><span class="keyword">\</span><span class="default">ns3</span><span class="keyword">;<br><br>class </span><span class="default">A </span><span class="keyword">{}<br><br></span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;ns1\ns2\ns3\A&apos;</span><span class="keyword">, </span><span class="string">&apos;B&apos;</span><span class="keyword">);<br></span><span class="comment">/* or if you want B to exist in ns1\ns2\ns3 */<br></span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;ns1\ns2\ns3\A&apos;</span><span class="keyword">, </span><span class="string">&apos;ns1\ns2\ns3\B&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
class_alias also works for interfaces!<br><br><span class="default">&lt;?php<br></span><span class="keyword">interface </span><span class="default">foo </span><span class="keyword">{}<br></span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;foo&apos;</span><span class="keyword">, </span><span class="string">&apos;bar&apos;</span><span class="keyword">);<br>echo </span><span class="default">interface_exists</span><span class="keyword">(</span><span class="string">&apos;bar&apos;</span><span class="keyword">) ? </span><span class="string">&apos;yes!&apos; </span><span class="keyword">: </span><span class="string">&apos;no&apos;</span><span class="keyword">; </span><span class="comment">// prints yes!<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.class-alias.php)

**[To root](/README.md)**