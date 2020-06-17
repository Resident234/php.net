# DateTime::__wakeup




<div class="phpcode"><span class="html">
If you use a version prior to 5.3 you can make __wakeup and __toString work using the following piece of code.<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">ExtendedDateTime </span><span class="keyword">extends </span><span class="default">DateTime </span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$_date_time</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">__toString</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">); </span><span class="comment">// format as ISO 8601<br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">__sleep</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_date_time </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return array(</span><span class="string">&apos;_date_time&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">__wakeup</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_date_time</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>Hope this helps someone.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.wakeup.php)

**[To root](/README.md)**