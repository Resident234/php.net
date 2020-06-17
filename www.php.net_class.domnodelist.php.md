# The DOMNodeList class




<div class="phpcode"><span class="html">
If you want to recurse over a DOM then this might help: <br><span class="default">&lt;?php <br><br></span><span class="comment">/**<br> * PHP&apos;s DOM classes are recursive but don&apos;t provide an implementation of <br> * RecursiveIterator. This class provides a RecursiveIterator for looping over DOMNodeList<br> */<br></span><span class="keyword">class </span><span class="default">DOMNodeRecursiveIterator </span><span class="keyword">extends </span><span class="default">ArrayIterator </span><span class="keyword">implements </span><span class="default">RecursiveIterator </span><span class="keyword">{<br>&#xA0; <br>&#xA0; public function </span><span class="default">__construct </span><span class="keyword">(</span><span class="default">DOMNodeList $node_list</span><span class="keyword">) {<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$nodes </span><span class="keyword">= array();<br>&#xA0; &#xA0; foreach(</span><span class="default">$node_list </span><span class="keyword">as </span><span class="default">$node</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$nodes</span><span class="keyword">[] = </span><span class="default">$node</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$nodes</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; }<br>&#xA0; <br>&#xA0; public function </span><span class="default">getRecursiveIterator</span><span class="keyword">(){<br>&#xA0; &#xA0; return new </span><span class="default">RecursiveIteratorIterator</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, </span><span class="default">RecursiveIteratorIterator</span><span class="keyword">::</span><span class="default">SELF_FIRST</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; <br>&#xA0; public function </span><span class="default">hasChildren </span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">()-&gt;</span><span class="default">hasChildNodes</span><span class="keyword">();<br>&#xA0; }<br><br>&#xA0; <br>&#xA0; public function </span><span class="default">getChildren </span><span class="keyword">() {<br>&#xA0; &#xA0; return new </span><span class="default">self</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">()-&gt;</span><span class="default">childNodes</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; <br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can modify, and even delete, nodes from a DOMNodeList if you iterate backwards:<br><br>$els = $document-&gt;getElementsByTagName(&apos;input&apos;);<br>for ($i = $els-&gt;length; --$i &gt;= 0; ) {<br>&#xA0; $el = $els-&gt;item($i);<br>&#xA0; switch ($el-&gt;getAttribute(&apos;name&apos;)) {<br>&#xA0; &#xA0; case &apos;MAX_FILE_SIZE&apos; :<br>&#xA0; &#xA0; &#xA0; $el-&gt;parentNode-&gt;removeChild($el);<br>&#xA0; &#xA0; break;<br>&#xA0; &#xA0; case &apos;inputfile&apos; :<br>&#xA0; &#xA0; &#xA0; $el-&gt;setAttribute(&apos;type&apos;, &apos;text&apos;);<br>&#xA0; &#xA0; //break;<br>&#xA0; }<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that $length is calculated (php5.3.2).<br>Iterating over a large NodeList may be time expensive.<br><br>Prefer :<br>$nb = $nodelist-&gt;length;<br>for($pos=0; $pos&lt;$nb; $pos++)<br><br>Than:<br>for($pos=0; $pos&lt;$nodelist-&gt;length; $pos++)<br><br>I had a hard time figuring that out...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.domnodelist.php)

**[To root](/README.md)**