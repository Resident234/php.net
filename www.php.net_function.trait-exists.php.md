# trait_exists




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="keyword">trait </span><span class="default">World </span><span class="keyword">{<br><br>&#xA0; &#xA0; private static </span><span class="default">$instance</span><span class="keyword">;<br>&#xA0; &#xA0; protected </span><span class="default">$tmp</span><span class="keyword">;<br><br>&#xA0; &#xA0; public static function </span><span class="default">World</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$instance </span><span class="keyword">= new static();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$instance</span><span class="keyword">-&gt;</span><span class="default">tmp </span><span class="keyword">= </span><span class="default">get_called_class</span><span class="keyword">().</span><span class="string">&apos; &apos;</span><span class="keyword">.</span><span class="default">__TRAIT__</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">self</span><span class="keyword">::</span><span class="default">$instance</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br>if ( </span><span class="default">trait_exists</span><span class="keyword">( </span><span class="string">&apos;World&apos; </span><span class="keyword">) ) {<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; class </span><span class="default">Hello </span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; use </span><span class="default">World</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">text</span><span class="keyword">( </span><span class="default">$str </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">tmp</span><span class="keyword">.</span><span class="default">$str</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>}<br><br>echo </span><span class="default">Hello</span><span class="keyword">::</span><span class="default">World</span><span class="keyword">()-&gt;</span><span class="default">text</span><span class="keyword">(</span><span class="string">&apos;!!!&apos;</span><span class="keyword">); </span><span class="comment">// Hello World!!!</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.trait-exists.php)

**[To root](/README.md)**