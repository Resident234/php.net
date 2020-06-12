# SplPriorityQueue::compare




<div class="phpcode"><span class="html">
At this time, the documentation sais &quot;Note: Multiple elements with the same priority will get dequeued in no particular order.&quot;<br><br>If you need elements of equal priority to maintain insertion order, you can use something like:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">StablePriorityQueue </span><span class="keyword">extends </span><span class="default">SplPriorityQueue </span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$serial </span><span class="keyword">= </span><span class="default">PHP_INT_MAX</span><span class="keyword">;<br>&#xA0; &#xA0; public function </span><span class="default">insert</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$priority</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">insert</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, array(</span><span class="default">$priority</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">serial</span><span class="keyword">--));<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/splpriorityqueue.compare.php)

**[⬆ to root](/)**