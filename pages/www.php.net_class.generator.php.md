# The Generator class




<div class="phpcode"><span class="html">
Unlike return, yield can be used anywhere within a function so logic can flow more naturally. Take for example the following Fibonacci generator:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">fib</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$cur </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$prev </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$n</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; yield </span><span class="default">$cur</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$temp </span><span class="keyword">= </span><span class="default">$cur</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cur </span><span class="keyword">= </span><span class="default">$prev </span><span class="keyword">+ </span><span class="default">$cur</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$prev </span><span class="keyword">= </span><span class="default">$temp</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$fibs </span><span class="keyword">= </span><span class="default">fib</span><span class="keyword">(</span><span class="default">9</span><span class="keyword">);<br>foreach (</span><span class="default">$fibs </span><span class="keyword">as </span><span class="default">$fib</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot; &quot; </span><span class="keyword">. </span><span class="default">$fib</span><span class="keyword">;<br>}<br><br></span><span class="comment">// prints: 1 1 2 3 5 8 13 21 34</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.generator.php)

**[To root](/README.md)**