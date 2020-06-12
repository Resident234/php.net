# The SplPriorityQueue class




<div class="phpcode"><span class="html">
quick implementation of SPL Priority Queue:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">PQtest </span><span class="keyword">extends </span><span class="default">SplPriorityQueue
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">compare</span><span class="keyword">(</span><span class="default">$priority1</span><span class="keyword">, </span><span class="default">$priority2</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$priority1 </span><span class="keyword">=== </span><span class="default">$priority2</span><span class="keyword">) return </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$priority1 </span><span class="keyword">&lt; </span><span class="default">$priority2 </span><span class="keyword">? -</span><span class="default">1 </span><span class="keyword">: </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$objPQ </span><span class="keyword">= new </span><span class="default">PQtest</span><span class="keyword">();
<br>
<br></span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">insert</span><span class="keyword">(</span><span class="string">&apos;A&apos;</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">);
<br></span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">insert</span><span class="keyword">(</span><span class="string">&apos;B&apos;</span><span class="keyword">,</span><span class="default">6</span><span class="keyword">);
<br></span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">insert</span><span class="keyword">(</span><span class="string">&apos;C&apos;</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">insert</span><span class="keyword">(</span><span class="string">&apos;D&apos;</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">);
<br>
<br>echo </span><span class="string">&quot;COUNT-&gt;&quot;</span><span class="keyword">.</span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">count</span><span class="keyword">().</span><span class="string">&quot;&lt;BR&gt;&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment">//mode of extraction
<br></span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">setExtractFlags</span><span class="keyword">(</span><span class="default">PQtest</span><span class="keyword">::</span><span class="default">EXTR_BOTH</span><span class="keyword">);
<br>
<br></span><span class="comment">//Go to TOP
<br></span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">top</span><span class="keyword">();
<br>
<br>while(</span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">valid</span><span class="keyword">()){
<br>&#xA0; &#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">());
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;BR&gt;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$objPQ</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">();
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>output:
<br>
<br>COUNT-&gt;4
<br>Array ( [data] =&gt; B [priority] =&gt; 6 ) 
<br>Array ( [data] =&gt; A [priority] =&gt; 3 ) 
<br>Array ( [data] =&gt; D [priority] =&gt; 2 ) 
<br>Array ( [data] =&gt; C [priority] =&gt; 1 )</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.splpriorityqueue.php)

**[⬆ to root](/)**