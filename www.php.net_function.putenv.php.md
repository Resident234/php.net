# putenv




<div class="phpcode"><span class="html">
putenv/getenv, $_ENV, and phpinfo(INFO_ENVIRONMENT) are three completely distinct environment stores. doing putenv(&quot;x=y&quot;) does not affect $_ENV; but also doing $_ENV[&quot;x&quot;]=&quot;y&quot; likewise does not affect getenv(&quot;x&quot;). And neither affect what is returned in phpinfo().<br><br>Assuming the USER environment variable is defined as &quot;dave&quot; before running the following:<br><br><span class="default">&lt;?php<br></span><span class="keyword">print </span><span class="string">&quot;env is: &quot;</span><span class="keyword">.</span><span class="default">$_ENV</span><span class="keyword">[</span><span class="string">&quot;USER&quot;</span><span class="keyword">].</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>print </span><span class="string">&quot;(doing: putenv fred)\n&quot;</span><span class="keyword">;<br></span><span class="default">putenv</span><span class="keyword">(</span><span class="string">&quot;USER=fred&quot;</span><span class="keyword">);<br>print </span><span class="string">&quot;env is: &quot;</span><span class="keyword">.</span><span class="default">$_ENV</span><span class="keyword">[</span><span class="string">&quot;USER&quot;</span><span class="keyword">].</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>print </span><span class="string">&quot;getenv is: &quot;</span><span class="keyword">.</span><span class="default">getenv</span><span class="keyword">(</span><span class="string">&quot;USER&quot;</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>print </span><span class="string">&quot;(doing: set _env barney)\n&quot;</span><span class="keyword">;<br></span><span class="default">$_ENV</span><span class="keyword">[</span><span class="string">&quot;USER&quot;</span><span class="keyword">]=</span><span class="string">&quot;barney&quot;</span><span class="keyword">;<br>print </span><span class="string">&quot;getenv is: &quot;</span><span class="keyword">.</span><span class="default">getenv</span><span class="keyword">(</span><span class="string">&quot;USER&quot;</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>print </span><span class="string">&quot;env is: &quot;</span><span class="keyword">.</span><span class="default">$_ENV</span><span class="keyword">[</span><span class="string">&quot;USER&quot;</span><span class="keyword">].</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">phpinfo</span><span class="keyword">(</span><span class="default">INFO_ENVIRONMENT</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>prints:<br><br>env is: dave<br>(doing: putenv fred)<br>env is: dave<br>getenv is: fred<br>(doing: set _env barney)<br>getenv is: fred<br>env is: barney<br>phpinfo()<br><br>Environment<br><br>Variable =&gt; Value<br>...<br>USER =&gt; dave<br>...</span>
</div>
  

#


<div class="phpcode"><span class="html">
The other problem with the code from av01 at bugfix dot cc is that<br>the behaviour is as per the comments here, not there:<br><span class="default">&lt;?php<br>putenv</span><span class="keyword">(</span><span class="string">&apos;MYVAR=&apos;</span><span class="keyword">); </span><span class="comment">// set MYVAR to an empty value.&#xA0; It is in the environment<br></span><span class="default">putenv</span><span class="keyword">(</span><span class="string">&apos;MYVAR&apos;</span><span class="keyword">); </span><span class="comment">// unset MYVAR.&#xA0; It is removed from the environment<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.putenv.php)

**[To root](/README.md)**