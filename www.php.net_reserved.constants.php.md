# Predefined Constants




<div class="phpcode"><span class="html">
Volker&apos;s getOS() function needs to have the order of cases changed in the switch statement since &quot;darwin&quot; contains &quot;win&quot;, which means that both &quot;windows&quot; and &quot;darwin&quot; will return self::OS_WIN. I&apos;ve moved the &apos;dar&apos; case above the &apos;win&apos; case:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">System </span><span class="keyword">{<br><br>&#xA0; &#xA0; const </span><span class="default">OS_UNKNOWN </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; const </span><span class="default">OS_WIN </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;<br>&#xA0; &#xA0; const </span><span class="default">OS_LINUX </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">;<br>&#xA0; &#xA0; const </span><span class="default">OS_OSX </span><span class="keyword">= </span><span class="default">4</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * @return int<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">static public function </span><span class="default">getOS</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; switch (</span><span class="default">true</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">stristr</span><span class="keyword">(</span><span class="default">PHP_OS</span><span class="keyword">, </span><span class="string">&apos;DAR&apos;</span><span class="keyword">): return </span><span class="default">self</span><span class="keyword">::</span><span class="default">OS_OSX</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">stristr</span><span class="keyword">(</span><span class="default">PHP_OS</span><span class="keyword">, </span><span class="string">&apos;WIN&apos;</span><span class="keyword">): return </span><span class="default">self</span><span class="keyword">::</span><span class="default">OS_WIN</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">stristr</span><span class="keyword">(</span><span class="default">PHP_OS</span><span class="keyword">, </span><span class="string">&apos;LINUX&apos;</span><span class="keyword">): return </span><span class="default">self</span><span class="keyword">::</span><span class="default">OS_LINUX</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; default : return </span><span class="default">self</span><span class="keyword">::</span><span class="default">OS_UNKNOWN</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.constants.php)

**[To root](/README.md)**