# The Countable interface




<div class="phpcode"><span class="html">
I just want to point out that your class has to actually implement the Countable interface, not just define a count method, to be able to use count($object) and get the expected results. I.e. the first example below won&apos;t work as expected, the second will. (The normal arrow function accessor ($object-&gt;count()) will work fine, but that&apos;s not the kewl part :) )
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">//Example One, BAD :(
<br>
<br></span><span class="keyword">class </span><span class="default">CountMe
<br></span><span class="keyword">{
<br>
<br>&#xA0; &#xA0; protected </span><span class="default">$_myCount </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">count</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_myCount</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>}
<br>
<br></span><span class="default">$countable </span><span class="keyword">= new </span><span class="default">CountMe</span><span class="keyword">();
<br>echo </span><span class="default">count</span><span class="keyword">(</span><span class="default">$countable</span><span class="keyword">); </span><span class="comment">//result is &quot;1&quot;, not as expected
<br>
<br>//Example Two, GOOD :)
<br>
<br></span><span class="keyword">class </span><span class="default">CountMe </span><span class="keyword">implements </span><span class="default">Countable
<br></span><span class="keyword">{
<br>
<br>&#xA0; &#xA0; protected </span><span class="default">$_myCount </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">count</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_myCount</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>}
<br>
<br></span><span class="default">$countable </span><span class="keyword">= new </span><span class="default">CountMe</span><span class="keyword">();
<br>echo </span><span class="default">count</span><span class="keyword">(</span><span class="default">$countable</span><span class="keyword">); </span><span class="comment">//result is &quot;3&quot; as expected
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.countable.php)

**[To root](/README.md)**