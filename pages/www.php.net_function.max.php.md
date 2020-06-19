# max




<div class="phpcode"><span class="html">
The simplest way to get around the fact that max() won&apos;t give the key is array_search:<br><br><span class="default">&lt;?php<br>$student_grades </span><span class="keyword">= array (</span><span class="string">&quot;john&quot; </span><span class="keyword">=&gt; </span><span class="default">100</span><span class="keyword">, </span><span class="string">&quot;sarah&quot; </span><span class="keyword">=&gt; </span><span class="default">90</span><span class="keyword">, </span><span class="string">&quot;anne&quot; </span><span class="keyword">=&gt; </span><span class="default">100</span><span class="keyword">);<br></span><span class="default">$top_student </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="default">max</span><span class="keyword">(</span><span class="default">$student_grades</span><span class="keyword">),</span><span class="default">$student_grades</span><span class="keyword">); </span><span class="comment">// john<br></span><span class="default">?&gt;<br></span><br>This could also be done with array_flip, though overwriting will mean that it gets the last max value rather than the first:<br><br><span class="default">&lt;?php<br>$grades_index </span><span class="keyword">= </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$student_grades</span><span class="keyword">);<br></span><span class="default">$top_student </span><span class="keyword">= </span><span class="default">$grades_index</span><span class="keyword">[</span><span class="default">max</span><span class="keyword">(</span><span class="default">$student_grades</span><span class="keyword">)]; </span><span class="comment">// anne<br></span><span class="default">?&gt;<br></span><br>To get all the max value keys:<br><br><span class="default">&lt;?php<br>$top_students </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$student_grades</span><span class="keyword">,</span><span class="default">max</span><span class="keyword">(</span><span class="default">$student_grades</span><span class="keyword">)); </span><span class="comment">// john, anne<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
max(null, 0) = null
<br>max(0, null) = 0</span>
</div>
  

#


<div class="phpcode"><span class="html">
max() (and min()) on DateTime objects compares them like dates (with timezone info) and returns DateTime object.<br><span class="default">&lt;?php <br>$dt1 </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;2014-05-07 18:53&apos;</span><span class="keyword">, new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;Europe/Kiev&apos;</span><span class="keyword">));<br></span><span class="default">$dt2 </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;2014-05-07 16:53&apos;</span><span class="keyword">, new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;UTC&apos;</span><span class="keyword">));<br>echo </span><span class="default">max</span><span class="keyword">(</span><span class="default">$dt1</span><span class="keyword">,</span><span class="default">$dt2</span><span class="keyword">)-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">RFC3339</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">; </span><span class="comment">// 2014-05-07T16:53:00+00:00<br></span><span class="keyword">echo </span><span class="default">min</span><span class="keyword">(</span><span class="default">$dt1</span><span class="keyword">,</span><span class="default">$dt2</span><span class="keyword">)-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">RFC3339</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">; </span><span class="comment">// 2014-05-07T18:53:00+03:00<br></span><span class="default">?&gt;<br></span><br>It works at least 5.3.3-7+squeeze17</span>
</div>
  

#


<div class="phpcode"><span class="html">
Notice that whenever there is a Number in front of the String, it will be used for Comparison.<br><br><span class="default">&lt;?php<br><br>&#xA0; max</span><span class="keyword">(</span><span class="string">&apos;7iuwmssuxue&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">); </span><span class="comment">//returns 7iuwmssuxu<br>&#xA0; </span><span class="default">max</span><span class="keyword">(</span><span class="string">&apos;-7suidha&apos;</span><span class="keyword">, -</span><span class="default">4</span><span class="keyword">); </span><span class="comment">//returns -4<br><br></span><span class="default">?&gt;<br></span><br>But just if it is in front of the String<br><br><span class="default">&lt;?php<br><br>&#xA0; max</span><span class="keyword">(</span><span class="string">&apos;sdihatewin7wduiw&apos;</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">); </span><span class="comment">//returns 3<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.max.php)

**[To root](/README.md)**