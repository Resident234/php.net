# implode




<div class="phpcode"><span class="html">
it should be noted that an array with one or no elements works fine. for example:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $a1 </span><span class="keyword">= array(</span><span class="string">&quot;1&quot;</span><span class="keyword">,</span><span class="string">&quot;2&quot;</span><span class="keyword">,</span><span class="string">&quot;3&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$a2 </span><span class="keyword">= array(</span><span class="string">&quot;a&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$a3 </span><span class="keyword">= array();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; echo </span><span class="string">&quot;a1 is: &apos;&quot;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&apos;,&apos;&quot;</span><span class="keyword">,</span><span class="default">$a1</span><span class="keyword">).</span><span class="string">&quot;&apos;&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="string">&quot;a2 is: &apos;&quot;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&apos;,&apos;&quot;</span><span class="keyword">,</span><span class="default">$a2</span><span class="keyword">).</span><span class="string">&quot;&apos;&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="string">&quot;a3 is: &apos;&quot;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&apos;,&apos;&quot;</span><span class="keyword">,</span><span class="default">$a3</span><span class="keyword">).</span><span class="string">&quot;&apos;&lt;br&gt;&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>will produce:<br>===========<br>a1 is: &apos;1&apos;,&apos;2&apos;,&apos;3&apos;<br>a2 is: &apos;a&apos;<br>a3 is: &apos;&apos;</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s not obvious from the samples, if/how associative arrays are handled. The &quot;implode&quot; function acts on the array &quot;values&quot;, disregarding any keys:<br><br><span class="default">&lt;?php<br></span><span class="keyword">declare(</span><span class="default">strict_types</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">);<br><br></span><span class="default">$a </span><span class="keyword">= array( </span><span class="string">&apos;one&apos;</span><span class="keyword">,</span><span class="string">&apos;two&apos;</span><span class="keyword">,</span><span class="string">&apos;three&apos; </span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= array( </span><span class="string">&apos;1st&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;four&apos;</span><span class="keyword">, </span><span class="string">&apos;five&apos;</span><span class="keyword">, </span><span class="string">&apos;3rd&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;six&apos; </span><span class="keyword">);<br><br>echo </span><span class="default">implode</span><span class="keyword">( </span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$a </span><span class="keyword">),</span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="default">implode</span><span class="keyword">( </span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>outputs:<br>one,two,three/four,five,six</span>
</div>
  

#


<div class="phpcode"><span class="html">
Can also be used for building tags or complex lists, like the following:<br><br><span class="default">&lt;?php<br><br>$elements </span><span class="keyword">= array(</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="string">&apos;c&apos;</span><span class="keyword">);<br><br>echo </span><span class="string">&quot;&lt;ul&gt;&lt;li&gt;&quot; </span><span class="keyword">. </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&lt;/li&gt;&lt;li&gt;&quot;</span><span class="keyword">, </span><span class="default">$elements</span><span class="keyword">) . </span><span class="string">&quot;&lt;/li&gt;&lt;/ul&gt;&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>This is just an example, you can create a lot more just finding the right glue! ;)</span>
</div>
  

#


<div class="phpcode"><span class="html">
It might be worthwhile noting that the array supplied to implode() can contain objects, provided the objects implement the __toString() method.<br><br>Example:<br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Foo<br></span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$title</span><span class="keyword">;<br><br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$title</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">title </span><span class="keyword">= </span><span class="default">$title</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">__toString</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">title</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$array </span><span class="keyword">= [<br>&#xA0; &#xA0; new </span><span class="default">Foo</span><span class="keyword">(</span><span class="string">&apos;foo&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; new </span><span class="default">Foo</span><span class="keyword">(</span><span class="string">&apos;bar&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; new </span><span class="default">Foo</span><span class="keyword">(</span><span class="string">&apos;qux&apos;</span><span class="keyword">)<br>];<br><br>echo </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;; &apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>will output:<br><br>foo; bar; qux</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to implode an array of booleans, you will get a strange result:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;&apos;</span><span class="keyword">,array(</span><span class="default">true</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">)));<br></span><span class="default">?&gt;<br></span><br>Output:<br>string(3) &quot;111&quot;<br><br>TRUE became &quot;1&quot;, FALSE became nothing.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Also quite handy in INSERT statements:<br><br><span class="default">&lt;?php<br><br>&#xA0;&#xA0; </span><span class="comment">// array containing data<br>&#xA0;&#xA0; </span><span class="default">$array </span><span class="keyword">= array(<br>&#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;John&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; </span><span class="string">&quot;surname&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;Doe&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; </span><span class="string">&quot;email&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;j.doe@intelligence.gov&quot;<br>&#xA0;&#xA0; </span><span class="keyword">);<br><br>&#xA0;&#xA0; </span><span class="comment">// build query...<br>&#xA0;&#xA0; </span><span class="default">$sql&#xA0; </span><span class="keyword">= </span><span class="string">&quot;INSERT INTO table&quot;</span><span class="keyword">;<br><br>&#xA0;&#xA0; </span><span class="comment">// implode keys of $array...<br>&#xA0;&#xA0; </span><span class="default">$sql </span><span class="keyword">.= </span><span class="string">&quot; (`&quot;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;`, `&quot;</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)).</span><span class="string">&quot;`)&quot;</span><span class="keyword">;<br><br>&#xA0;&#xA0; </span><span class="comment">// implode values of $array...<br>&#xA0;&#xA0; </span><span class="default">$sql </span><span class="keyword">.= </span><span class="string">&quot; VALUES (&apos;&quot;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&apos;, &apos;&quot;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">).</span><span class="string">&quot;&apos;) &quot;</span><span class="keyword">;<br><br>&#xA0;&#xA0; </span><span class="comment">// execute query...<br>&#xA0;&#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">mysql_query</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">) or die(</span><span class="default">mysql_error</span><span class="keyword">());<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It may be worth noting that if you accidentally call implode on a string rather than an array, you do NOT get your string back, you get NULL:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;:&apos;</span><span class="keyword">, </span><span class="string">&apos;xxxxx&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span>returns<br>NULL<br><br>This threw me for a little while.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Even handier if you use the following:
<br>
<br><span class="default">&lt;?php
<br>$id_nums </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">,</span><span class="default">6</span><span class="keyword">,</span><span class="default">12</span><span class="keyword">,</span><span class="default">18</span><span class="keyword">,</span><span class="default">24</span><span class="keyword">);
<br>
<br></span><span class="default">$id_nums </span><span class="keyword">= </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;, &quot;</span><span class="keyword">, </span><span class="default">$id_nums</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="default">$sqlquery </span><span class="keyword">= </span><span class="string">&quot;Select name,email,phone from usertable where user_id IN (</span><span class="default">$id_nums</span><span class="string">)&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment">// $sqlquery becomes &quot;Select name,email,phone from usertable where user_id IN (1,6,12,18,24)&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>Be sure to escape/sanitize/use prepared statements if you get the ids from users.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.implode.php)

**[⬆ to root](/)**