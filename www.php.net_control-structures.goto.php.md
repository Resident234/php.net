# goto




<div class="phpcode"><span class="html">
Remember if you are not a fan of wild labels hanging around you are free to use braces in this construct creating a slightly cleaner look. Labels also are always executed and do not need to be called to have their associated code block ran. A purposeless example is below.
<br>
<br><span class="default">&lt;?php
<br>
<br>$headers </span><span class="keyword">= Array(</span><span class="string">&apos;subject&apos;</span><span class="keyword">, </span><span class="string">&apos;bcc&apos;</span><span class="keyword">, </span><span class="string">&apos;to&apos;</span><span class="keyword">, </span><span class="string">&apos;cc&apos;</span><span class="keyword">, </span><span class="string">&apos;date&apos;</span><span class="keyword">, </span><span class="string">&apos;sender&apos;</span><span class="keyword">);
<br></span><span class="default">$position </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br></span><span class="default">hIterator</span><span class="keyword">: {
<br>
<br>&#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; echo </span><span class="default">$headers</span><span class="keyword">[</span><span class="default">$position</span><span class="keyword">] . </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; </span><span class="default">cIterator</span><span class="keyword">: {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos; &apos; </span><span class="keyword">. </span><span class="default">$headers</span><span class="keyword">[</span><span class="default">$position</span><span class="keyword">][</span><span class="default">$c</span><span class="keyword">] . </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!isset(</span><span class="default">$headers</span><span class="keyword">[</span><span class="default">$position</span><span class="keyword">][++</span><span class="default">$c</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; goto </span><span class="default">cIteratorExit</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; goto </span><span class="default">cIterator</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">cIteratorExit</span><span class="keyword">: {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(isset(</span><span class="default">$headers</span><span class="keyword">[++</span><span class="default">$position</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; goto </span><span class="default">hIterator</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You cannot implement a Fortran-style &quot;computed GOTO&quot; in PHP because the label cannot be a variable. See: <a href="http://en.wikipedia.org/wiki/Considered_harmful" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Considered_harmful</a>
<br>
<br><span class="default">&lt;?php </span><span class="comment">// RAY_goto.php
<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);
<br>
<br></span><span class="comment">// DEMONSTRATE THAT THE GOTO LABEL IS CASE-SENSITIVE
<br>
<br></span><span class="keyword">goto </span><span class="default">a</span><span class="keyword">;
<br>echo </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;
<br></span><span class="default">a</span><span class="keyword">: echo </span><span class="string">&apos;Bar&apos;</span><span class="keyword">;
<br>
<br>goto </span><span class="default">A</span><span class="keyword">;
<br>echo </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;
<br></span><span class="default">A</span><span class="keyword">: echo </span><span class="string">&apos;Baz&apos;</span><span class="keyword">;
<br>
<br></span><span class="comment">// CAN THE GOTO LABEL BE A VARIABLE?
<br>
<br></span><span class="default">$a </span><span class="keyword">= </span><span class="string">&apos;abc&apos;</span><span class="keyword">;
<br>goto </span><span class="default">$a</span><span class="keyword">; </span><span class="comment">// NOPE: PARSE ERROR
<br></span><span class="keyword">echo </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;
<br></span><span class="default">abc</span><span class="keyword">: echo </span><span class="string">&apos;Boom&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
However hated, goto is useful. When we say &quot;useful&quot; we don&apos;t mean &quot;it should be used all the time&quot; but that there are certain situations when it comes in handy.<br><br>There are times when you need a logical structure like this:<br><span class="default">&lt;?php<br></span><span class="comment">// ...<br></span><span class="keyword">do {<br><br>&#xA0; &#xA0; </span><span class="default">$answer </span><span class="keyword">= </span><span class="default">checkFirstSource</span><span class="keyword">();<br>&#xA0; &#xA0; if(</span><span class="default">seemsGood</span><span class="keyword">(</span><span class="default">$answer</span><span class="keyword">)) break;<br><br>&#xA0; &#xA0; </span><span class="default">$answer </span><span class="keyword">= </span><span class="default">readFromAnotherSource</span><span class="keyword">();<br>&#xA0; &#xA0; if(</span><span class="default">seemsGood</span><span class="keyword">(</span><span class="default">$answer</span><span class="keyword">)) break;<br><br>&#xA0; &#xA0; </span><span class="comment">// ...<br><br></span><span class="keyword">}while(</span><span class="default">0</span><span class="keyword">);<br></span><span class="default">$answer </span><span class="keyword">= </span><span class="default">applyFinalTouches</span><span class="keyword">(</span><span class="default">$answer</span><span class="keyword">);<br>return </span><span class="default">$answer</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>In this case, you certainly implemented a goto with a &quot;fake loop pattern&quot;.&#xA0; It could be a lot more readable with a goto; unless, of course, you hate it.&#xA0; But the logic is clear: try everything you can to get $answer, and whenever it seems good (e.g. not empty), jump happily to the point where you format it and give it back to the caller.&#xA0; It&apos;s a proper implementation of a simple fallback mechanism.<br><br>Basically, the fight against goto is just a side effect of a misleading article many decades ago.&#xA0; Those monsters are gone now.&#xA0; Feel free to use it when you know what you&apos;re doing.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The goto operator CAN be evaluated with eval, provided the label is in the eval&apos;d code:<br><br><span class="default">&lt;?php<br>a</span><span class="keyword">: eval(</span><span class="string">&quot;goto a;&quot;</span><span class="keyword">); </span><span class="comment">// undefined label &apos;a&apos;<br></span><span class="keyword">eval(</span><span class="string">&quot;a: goto a;&quot;</span><span class="keyword">); </span><span class="comment">// works<br></span><span class="default">?&gt;<br></span><br>It&apos;s because PHP does not consider the eval&apos;d code, containing the label, to be in the same &quot;file&quot; as the goto statement.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You are also allowed to jump backwards with a goto statement. To run a block of goto as one block is as follows:<br>example has a prefix of iw_ to keep label groups structured and an extra underscore to do a backwards goto.<br><br>Note the `iw_end_gt` to get out of the labels area<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $link </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br><br>&#xA0; &#xA0; if ( </span><span class="default">$link </span><span class="keyword">) goto </span><span class="default">iw_link_begin</span><span class="keyword">; <br>&#xA0; &#xA0; if(</span><span class="default">false</span><span class="keyword">) </span><span class="default">iw__link_begin</span><span class="keyword">:<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if ( </span><span class="default">$link </span><span class="keyword">) goto </span><span class="default">iw_link_text</span><span class="keyword">;<br>&#xA0; &#xA0; if(</span><span class="default">false</span><span class="keyword">) </span><span class="default">iw__link_text</span><span class="keyword">:<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if ( </span><span class="default">$link </span><span class="keyword">) goto </span><span class="default">iw_link_end</span><span class="keyword">;<br>&#xA0; &#xA0; if(</span><span class="default">false</span><span class="keyword">) </span><span class="default">iw__link_end</span><span class="keyword">:<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; goto </span><span class="default">iw_end_gt</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (</span><span class="default">false</span><span class="keyword">) </span><span class="default">iw_link_begin</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;a href=&quot;#&quot;&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; goto </span><span class="default">iw__link_begin</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (</span><span class="default">false</span><span class="keyword">) </span><span class="default">iw_link_text</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Sample Text&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; goto </span><span class="default">iw__link_text</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (</span><span class="default">false</span><span class="keyword">) </span><span class="default">iw_link_end</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;/a&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; goto </span><span class="default">iw__link_end</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">iw_end_gt</span><span class="keyword">:<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.goto.php)

**[To root](/)**