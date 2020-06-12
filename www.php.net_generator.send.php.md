# Generator::send




<div class="phpcode"><span class="html">
Reading the example, it is a bit difficult to understand what exactly to do with this. The example below is a simple example of what you can do this.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">nums</span><span class="keyword">() {<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">5</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//get a value from the caller<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cmd </span><span class="keyword">= (yield </span><span class="default">$i</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$cmd </span><span class="keyword">== </span><span class="string">&apos;stop&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;</span><span class="comment">//exit the function<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}&#xA0; &#xA0;&#xA0; <br>}<br><br></span><span class="default">$gen </span><span class="keyword">= </span><span class="default">nums</span><span class="keyword">();<br>foreach(</span><span class="default">$gen </span><span class="keyword">as </span><span class="default">$v</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if(</span><span class="default">$v </span><span class="keyword">== </span><span class="default">3</span><span class="keyword">)</span><span class="comment">//we are satisfied<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$gen</span><span class="keyword">-&gt;</span><span class="default">send</span><span class="keyword">(</span><span class="string">&apos;stop&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$v</span><span class="keyword">}</span><span class="string">\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="comment">//Output<br></span><span class="default">0<br>1<br>2<br>3<br>?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/generator.send.php)

**[To root](/)**