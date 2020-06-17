# var_dump




<div class="phpcode"><span class="html">
Keep in mind if you have xdebug installed it will limit the var_dump() output of array elements and object properties to 3 levels deep.<br><br>To change the default, edit your xdebug.ini file and add the folllowing line:<br>xdebug.var_display_max_depth=n<br><br>More information here:<br><a href="http://www.xdebug.org/docs/display" rel="nofollow" target="_blank">http://www.xdebug.org/docs/display</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re like me and uses var_dump whenever you&apos;re debugging, you might find these two &quot;wrapper&quot; functions helpful.<br><br>This one automatically adds the PRE tags around the var_dump output so you get nice formatted arrays.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">var_dump_pre</span><span class="keyword">(</span><span class="default">$mixed </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;<br>&#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$mixed</span><span class="keyword">);<br>&#xA0; echo </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;<br>&#xA0; return </span><span class="default">null</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>This one returns the value of var_dump instead of outputting it.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">var_dump_ret</span><span class="keyword">(</span><span class="default">$mixed </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; </span><span class="default">ob_start</span><span class="keyword">();<br>&#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$mixed</span><span class="keyword">);<br>&#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="default">ob_get_contents</span><span class="keyword">();<br>&#xA0; </span><span class="default">ob_end_clean</span><span class="keyword">();<br>&#xA0; return </span><span class="default">$content</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>Fairly simple functions, but they&apos;re infinitely helpful (I use var_dump_pre() almost exclusively now).</span>
</div>
  

#


<div class="phpcode"><span class="html">
I post a new var_dump function with colors and collapse features. It can also adapt to terminal output if you execute it from there. No need to wrap it in a pre tag to get it to work in browsers. <br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">dump_debug</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">$collapse</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$recursive </span><span class="keyword">= function(</span><span class="default">$data</span><span class="keyword">, </span><span class="default">$level</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">) use (&amp;</span><span class="default">$recursive</span><span class="keyword">, </span><span class="default">$collapse</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; global </span><span class="default">$argv</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$isTerminal </span><span class="keyword">= isset(</span><span class="default">$argv</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$isTerminal </span><span class="keyword">&amp;&amp; </span><span class="default">$level </span><span class="keyword">== </span><span class="default">0 </span><span class="keyword">&amp;&amp; !</span><span class="default">defined</span><span class="keyword">(</span><span class="string">&quot;DUMP_DEBUG_SCRIPT&quot;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">define</span><span class="keyword">(</span><span class="string">&quot;DUMP_DEBUG_SCRIPT&quot;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;script language=&quot;Javascript&quot;&gt;function toggleDisplay(id) {&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;var state = document.getElementById(&quot;container&quot;+id).style.display;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;document.getElementById(&quot;container&quot;+id).style.display = state == &quot;inline&quot; ? &quot;none&quot; : &quot;inline&quot;;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;document.getElementById(&quot;plus&quot;+id).style.display = state == &quot;inline&quot; ? &quot;inline&quot; : &quot;none&quot;;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;}&lt;/script&gt;&apos;</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type </span><span class="keyword">= !</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) &amp;&amp; </span><span class="default">is_callable</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) ? </span><span class="string">&quot;Callable&quot; </span><span class="keyword">: </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_data </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_color </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; switch (</span><span class="default">$type</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;String&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_color </span><span class="keyword">= </span><span class="string">&quot;green&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_data </span><span class="keyword">= </span><span class="string">&quot;\&quot;&quot; </span><span class="keyword">. </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) . </span><span class="string">&quot;\&quot;&quot;</span><span class="keyword">; break;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;Double&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;Float&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type </span><span class="keyword">= </span><span class="string">&quot;Float&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_color </span><span class="keyword">= </span><span class="string">&quot;#0099c5&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_data </span><span class="keyword">= </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">); break;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;Integer&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_color </span><span class="keyword">= </span><span class="string">&quot;red&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_data </span><span class="keyword">= </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">); break;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;Boolean&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_color </span><span class="keyword">= </span><span class="string">&quot;#92008d&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_data </span><span class="keyword">= </span><span class="default">$data </span><span class="keyword">? </span><span class="string">&quot;TRUE&quot; </span><span class="keyword">: </span><span class="string">&quot;FALSE&quot;</span><span class="keyword">; break;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;NULL&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; break;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&quot;Array&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type_length </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$type</span><span class="keyword">, array(</span><span class="string">&quot;Object&quot;</span><span class="keyword">, </span><span class="string">&quot;Array&quot;</span><span class="keyword">))) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$notEmpty </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$data </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$notEmpty</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$notEmpty </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$isTerminal</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$type </span><span class="keyword">. (</span><span class="default">$type_length </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="string">&quot;(&quot; </span><span class="keyword">. </span><span class="default">$type_length </span><span class="keyword">. </span><span class="string">&quot;)&quot; </span><span class="keyword">: </span><span class="string">&quot;&quot;</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$id </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">md5</span><span class="keyword">(</span><span class="default">rand</span><span class="keyword">().</span><span class="string">&quot;:&quot;</span><span class="keyword">.</span><span class="default">$key</span><span class="keyword">.</span><span class="string">&quot;:&quot;</span><span class="keyword">.</span><span class="default">$level</span><span class="keyword">), </span><span class="default">0</span><span class="keyword">, </span><span class="default">8</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;a href=\&quot;javascript:toggleDisplay(&apos;&quot;</span><span class="keyword">. </span><span class="default">$id </span><span class="keyword">.</span><span class="string">&quot;&apos;);\&quot; style=\&quot;text-decoration:none\&quot;&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;span style=&apos;color:#666666&apos;&gt;&quot; </span><span class="keyword">. </span><span class="default">$type </span><span class="keyword">. (</span><span class="default">$type_length </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="string">&quot;(&quot; </span><span class="keyword">. </span><span class="default">$type_length </span><span class="keyword">. </span><span class="string">&quot;)&quot; </span><span class="keyword">: </span><span class="string">&quot;&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;/span&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;/a&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;span id=\&quot;plus&quot;</span><span class="keyword">. </span><span class="default">$id </span><span class="keyword">.</span><span class="string">&quot;\&quot; style=\&quot;display: &quot; </span><span class="keyword">. (</span><span class="default">$collapse </span><span class="keyword">? </span><span class="string">&quot;inline&quot; </span><span class="keyword">: </span><span class="string">&quot;none&quot;</span><span class="keyword">) . </span><span class="string">&quot;;\&quot;&gt;&amp;nbsp;&amp;#10549;&lt;/span&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;div id=\&quot;container&quot;</span><span class="keyword">. </span><span class="default">$id </span><span class="keyword">.</span><span class="string">&quot;\&quot; style=\&quot;display: &quot; </span><span class="keyword">. (</span><span class="default">$collapse </span><span class="keyword">? </span><span class="string">&quot;&quot; </span><span class="keyword">: </span><span class="string">&quot;inline&quot;</span><span class="keyword">) . </span><span class="string">&quot;;\&quot;&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">$level</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="string">&quot;|&#xA0; &#xA0; &quot; </span><span class="keyword">: </span><span class="string">&quot;&lt;span style=&apos;color:black&apos;&gt;|&lt;/span&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="string">&quot;\n&quot; </span><span class="keyword">: </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">$level</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="string">&quot;|&#xA0; &#xA0; &quot; </span><span class="keyword">: </span><span class="string">&quot;&lt;span style=&apos;color:black&apos;&gt;|&lt;/span&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="string">&quot;[&quot; </span><span class="keyword">. </span><span class="default">$key </span><span class="keyword">. </span><span class="string">&quot;] =&gt; &quot; </span><span class="keyword">: </span><span class="string">&quot;&lt;span style=&apos;color:black&apos;&gt;[&quot; </span><span class="keyword">. </span><span class="default">$key </span><span class="keyword">. </span><span class="string">&quot;]&amp;nbsp;=&gt;&amp;nbsp;&lt;/span&gt;&quot;</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$recursive</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">, </span><span class="default">$level</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$notEmpty</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">$level</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="string">&quot;|&#xA0; &#xA0; &quot; </span><span class="keyword">: </span><span class="string">&quot;&lt;span style=&apos;color:black&apos;&gt;|&lt;/span&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$isTerminal</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;/div&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type </span><span class="keyword">. (</span><span class="default">$type_length </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="string">&quot;(&quot; </span><span class="keyword">. </span><span class="default">$type_length </span><span class="keyword">. </span><span class="string">&quot;)&quot; </span><span class="keyword">: </span><span class="string">&quot;&quot;</span><span class="keyword">) . </span><span class="string">&quot;&#xA0; &quot; </span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;span style=&apos;color:#666666&apos;&gt;&quot; </span><span class="keyword">. </span><span class="default">$type </span><span class="keyword">. (</span><span class="default">$type_length </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="string">&quot;(&quot; </span><span class="keyword">. </span><span class="default">$type_length </span><span class="keyword">. </span><span class="string">&quot;)&quot; </span><span class="keyword">: </span><span class="string">&quot;&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;/span&gt;&amp;nbsp;&amp;nbsp;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type </span><span class="keyword">. (</span><span class="default">$type_length </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="string">&quot;(&quot; </span><span class="keyword">. </span><span class="default">$type_length </span><span class="keyword">. </span><span class="string">&quot;)&quot; </span><span class="keyword">: </span><span class="string">&quot;&quot;</span><span class="keyword">) . </span><span class="string">&quot;&#xA0; &quot; </span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;span style=&apos;color:#666666&apos;&gt;&quot; </span><span class="keyword">. </span><span class="default">$type </span><span class="keyword">. (</span><span class="default">$type_length </span><span class="keyword">!== </span><span class="default">null </span><span class="keyword">? </span><span class="string">&quot;(&quot; </span><span class="keyword">. </span><span class="default">$type_length </span><span class="keyword">. </span><span class="string">&quot;)&quot; </span><span class="keyword">: </span><span class="string">&quot;&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;/span&gt;&amp;nbsp;&amp;nbsp;&quot;</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$type_data </span><span class="keyword">!= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="default">$type_data </span><span class="keyword">: </span><span class="string">&quot;&lt;span style=&apos;color:&quot; </span><span class="keyword">. </span><span class="default">$type_color </span><span class="keyword">. </span><span class="string">&quot;&apos;&gt;&quot; </span><span class="keyword">. </span><span class="default">$type_data </span><span class="keyword">. </span><span class="string">&quot;&lt;/span&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$isTerminal </span><span class="keyword">? </span><span class="string">&quot;\n&quot; </span><span class="keyword">: </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; };<br><br>&#xA0; &#xA0; </span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$recursive</span><span class="keyword">, </span><span class="default">$input</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.var-dump.php)

**[To root](/README.md)**