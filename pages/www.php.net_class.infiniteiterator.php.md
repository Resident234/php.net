# The InfiniteIterator class




<div class="phpcode"><span class="html">
to loop through object keys and reset to the start, try this:<br><span class="default">&lt;?php<br><br>$obj </span><span class="keyword">= new </span><span class="default">stdClass</span><span class="keyword">();<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Mon </span><span class="keyword">= </span><span class="string">&quot;Monday&quot;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Tue </span><span class="keyword">= </span><span class="string">&quot;Tuesday&quot;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Wed </span><span class="keyword">= </span><span class="string">&quot;Wednesday&quot;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Thu </span><span class="keyword">= </span><span class="string">&quot;Thursday&quot;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Fri </span><span class="keyword">= </span><span class="string">&quot;Friday&quot;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Sat </span><span class="keyword">= </span><span class="string">&quot;Saturday&quot;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">Sun </span><span class="keyword">= </span><span class="string">&quot;Sunday&quot;</span><span class="keyword">;<br><br></span><span class="default">$infinate </span><span class="keyword">= new </span><span class="default">InfiniteIterator</span><span class="keyword">(new </span><span class="default">ArrayIterator</span><span class="keyword">(</span><span class="default">$obj</span><span class="keyword">));<br>foreach ( new </span><span class="default">LimitIterator</span><span class="keyword">(</span><span class="default">$infinate</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">14</span><span class="keyword">) as </span><span class="default">$value </span><span class="keyword">) {<br>&#xA0; &#xA0; print(</span><span class="default">$value </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;<br></span><br>will output:<br><br>Monday<br>Tuesday<br>Wednesday<br>Thursday<br>Friday<br>Saturday<br>Sunday<br>Monday<br>Tuesday<br>Wednesday<br>Thursday<br>Friday<br>Saturday<br>Sunday<br><br>Can be useful when doing date operations or recurring events</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is important to realise that rewind() must be called on any iterator before using it or you may experience undefined behaviour, see example code and output here <a href="http://3v4l.org/rvNpU" rel="nofollow" target="_blank">http://3v4l.org/rvNpU</a><br><br>See this bug report <a href="https://bugs.php.net/bug.php?id=63823&amp;edit=2" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=63823&amp;edit=2</a> for a fuller explanation.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.infiniteiterator.php)

**[To root](/README.md)**