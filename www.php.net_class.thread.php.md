# The Thread class




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">workerThread </span><span class="keyword">extends </span><span class="default">Thread </span><span class="keyword">{<br> public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">){<br>&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">i</span><span class="keyword">=</span><span class="default">$i</span><span class="keyword">;<br> }<br><br> public function </span><span class="default">run</span><span class="keyword">(){<br>&#xA0; while(</span><span class="default">true</span><span class="keyword">){<br>&#xA0;&#xA0; echo </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">i</span><span class="keyword">;<br>&#xA0;&#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>&#xA0; }<br> }<br>}<br><br>for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">50</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">++){<br> </span><span class="default">$workers</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]=new </span><span class="default">workerThread</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">);<br> </span><span class="default">$workers</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]-&gt;</span><span class="default">start</span><span class="keyword">();<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.thread.php)

**[⬆ to root](/)**