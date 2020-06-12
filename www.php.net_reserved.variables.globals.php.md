# $GLOBALS




<div class="phpcode"><span class="html">
As of PHP 5.4 $GLOBALS is now initialized just-in-time. This means there now is an advantage to not use the $GLOBALS variable as you can avoid the overhead of initializing it. How much of an advantage that is I&apos;m not sure, but I&apos;ve never liked $GLOBALS much anyways.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Watch out when you are trying to set $GLOBALS to the local variable.<br><br>Even without reference operator &quot;&amp;&quot; your variable seems to be referenced to the $GLOBALS<br><br>You can test this behaviour using below code<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Result:<br> * POST: B, Variable: C<br> * GLOBALS: C, Variable: C<br> */<br> <br>// Testing $_POST<br></span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">] = </span><span class="string">&apos;B&apos;</span><span class="keyword">;<br> <br></span><span class="default">$nonReferencedPostVar </span><span class="keyword">= </span><span class="default">$_POST</span><span class="keyword">;<br></span><span class="default">$nonReferencedPostVar</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">] = </span><span class="string">&apos;C&apos;</span><span class="keyword">;<br> <br>echo </span><span class="string">&apos;POST: &apos;</span><span class="keyword">.</span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">].</span><span class="string">&apos;, Variable: &apos;</span><span class="keyword">.</span><span class="default">$nonReferencedPostVar</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">].</span><span class="string">&quot;\n\n&quot;</span><span class="keyword">;<br> <br></span><span class="comment">// Testing Globals<br></span><span class="default">$GLOBALS</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">] = </span><span class="string">&apos;B&apos;</span><span class="keyword">;<br> <br></span><span class="default">$nonReferencedGlobalsVar </span><span class="keyword">= </span><span class="default">$GLOBALS</span><span class="keyword">;<br></span><span class="default">$nonReferencedGlobalsVar</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">] = </span><span class="string">&apos;C&apos;</span><span class="keyword">;<br> <br>echo </span><span class="string">&apos;GLOBALS: &apos;</span><span class="keyword">.</span><span class="default">$GLOBALS</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">].</span><span class="string">&apos;, Variable: &apos;</span><span class="keyword">.</span><span class="default">$nonReferencedGlobalsVar</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">].</span><span class="string">&quot;\n\n&quot;</span><span class="keyword">;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.globals.php)

**[⬆ to root](/)**