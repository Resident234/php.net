# Name resolution rules




<div class="phpcode"><span class="html">
If you like to declare an __autoload function within a namespace or class, use the spl_autoload_register() function to register it and it will work fine.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The term &quot;autoload&quot; mentioned here shall not be confused with __autoload function to autoload objects. Regarding the __autoload and namespaces&apos; resolution I&apos;d like to share the following experience:<br><br>-&gt;Say you have the following directory structure:<br><br>- root<br>&#xA0; &#xA0; &#xA0; | - loader.php <br>&#xA0; &#xA0; &#xA0; | - ns<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | - foo.php<br><br>-&gt;foo.php<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">ns</span><span class="keyword">;<br>class </span><span class="default">foo<br></span><span class="keyword">{<br>&#xA0; &#xA0; public </span><span class="default">$say</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">say </span><span class="keyword">= </span><span class="string">&quot;bar&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>}<br></span><span class="default">?&gt;<br></span><br>-&gt; loader.php<br><br><span class="default">&lt;?php<br></span><span class="comment">//GLOBAL SPACE &lt;--<br></span><span class="keyword">function </span><span class="default">__autoload</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; require_once </span><span class="default">$c </span><span class="keyword">. </span><span class="string">&quot;.php&quot;</span><span class="keyword">;<br>}<br><br>class </span><span class="default">foo </span><span class="keyword">extends </span><span class="default">ns</span><span class="keyword">\</span><span class="default">foo </span><span class="comment">// ns\foo is loaded here<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;br /&gt;foo&quot; </span><span class="keyword">. </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">say</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">$a </span><span class="keyword">= new </span><span class="default">ns</span><span class="keyword">\</span><span class="default">foo</span><span class="keyword">(); </span><span class="comment">// ns\foo also loads ns/foo.php just fine here.<br></span><span class="keyword">echo </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">say</span><span class="keyword">;&#xA0;&#xA0; </span><span class="comment">// prints bar as expected.<br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">foo</span><span class="keyword">;&#xA0; </span><span class="comment">// prints foobar just fine.<br></span><span class="default">?&gt;<br></span><br>If you keep your directory/file matching namespace/class consistence the object __autoload works fine.<br>But... if you try to give loader.php a namespace you&apos;ll obviously get fatal errors. <br>My sample is just 1 level dir, but I&apos;ve tested with a very complex and deeper structure. Hope anybody finds this useful.<br><br>Cheers!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.rules.php)

**[⬆ to root](/)**