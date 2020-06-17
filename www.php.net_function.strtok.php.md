# strtok




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">// strtok example<br></span><span class="default">$str </span><span class="keyword">= </span><span class="string">&apos;Hello to all of Ukraine&apos;</span><span class="keyword">;<br>echo </span><span class="default">strtok</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="string">&apos; &apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">.</span><span class="default">strtok</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">.</span><span class="default">strtok</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>Result:<br>Hello to all</span>
</div>
  

#


<div class="phpcode"><span class="html">
&lt;pre&gt;<span class="default">&lt;?php<br></span><span class="comment">/** get leading, trailing, and embedded separator tokens that were &apos;skipped&apos;<br>if for some ungodly reason you are using php to implement a simple parser that <br>needs to detect nested clauses as it builds a parse tree */<br><br></span><span class="default">$str </span><span class="keyword">= </span><span class="string">&quot;(((alpha(beta))(gamma))&quot;</span><span class="keyword">;<br><br></span><span class="default">$seps </span><span class="keyword">= </span><span class="string">&apos;()&apos;</span><span class="keyword">;<br></span><span class="default">$tok </span><span class="keyword">= </span><span class="default">strtok</span><span class="keyword">( </span><span class="default">$str</span><span class="keyword">,</span><span class="default">$seps </span><span class="keyword">); </span><span class="comment">// return false on empty string or null<br></span><span class="default">$cur </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;&#xA0; &#xA0; &#xA0; <br></span><span class="default">$dumbDone </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">;<br></span><span class="default">$done </span><span class="keyword">= (</span><span class="default">FALSE</span><span class="keyword">===</span><span class="default">$tok</span><span class="keyword">);<br>while (!</span><span class="default">$done</span><span class="keyword">) {<br>&#xA0;&#xA0; </span><span class="comment">// process skipped tokens (if any at first iteration) (special for last)<br>&#xA0;&#xA0; </span><span class="default">$posTok </span><span class="keyword">= </span><span class="default">$dumbDone </span><span class="keyword">? </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) : </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">$tok</span><span class="keyword">, </span><span class="default">$cur </span><span class="keyword">);<br>&#xA0;&#xA0; </span><span class="default">$skippedMany </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">( </span><span class="default">$str</span><span class="keyword">, </span><span class="default">$cur</span><span class="keyword">, </span><span class="default">$posTok</span><span class="keyword">-</span><span class="default">$cur </span><span class="keyword">); </span><span class="comment">// false when 0 width<br>&#xA0;&#xA0; </span><span class="default">$lenSkipped </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$skippedMany</span><span class="keyword">); </span><span class="comment">// 0 when false<br>&#xA0;&#xA0; </span><span class="keyword">if (</span><span class="default">0</span><span class="keyword">!==</span><span class="default">$lenSkipped</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$last </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$skippedMany</span><span class="keyword">) -</span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">&lt;=</span><span class="default">$last</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++){<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$skipped </span><span class="keyword">= </span><span class="default">$skippedMany</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$cur </span><span class="keyword">+= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$skipped</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; echo </span><span class="string">&quot;skipped: </span><span class="default">$skipped</span><span class="string">\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0;&#xA0; }<br>&#xA0;&#xA0; if (</span><span class="default">$dumbDone</span><span class="keyword">) break; </span><span class="comment">// this is the only place the loop is terminated<br><br>&#xA0;&#xA0; // process current tok<br>&#xA0;&#xA0; </span><span class="keyword">echo </span><span class="string">&quot;curr tok: &quot;</span><span class="keyword">.</span><span class="default">$tok</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br>&#xA0;&#xA0; </span><span class="comment">// update cursor<br>&#xA0;&#xA0; </span><span class="default">$cur </span><span class="keyword">+= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$tok</span><span class="keyword">);<br><br>&#xA0;&#xA0; </span><span class="comment">// get any next tok<br>&#xA0;&#xA0; </span><span class="keyword">if (!</span><span class="default">$dumbDone</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$tok </span><span class="keyword">= </span><span class="default">strtok</span><span class="keyword">(</span><span class="default">$seps</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$dumbDone </span><span class="keyword">= (</span><span class="default">FALSE</span><span class="keyword">===</span><span class="default">$tok</span><span class="keyword">); <br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// you&apos;re not really done till you check for trailing skipped<br>&#xA0;&#xA0; </span><span class="keyword">}<br>};<br></span><span class="default">?&gt;</span>&lt;/pre&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strtok.php)

**[To root](/README.md)**