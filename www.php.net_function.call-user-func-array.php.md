# call_user_func_array




<div class="phpcode"><span class="html">
As of PHP 5.6 you can utilize argument unpacking as an alternative to call_user_func_array, and is often 3 to 4 times faster.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">foo </span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) {<br>&#xA0; &#xA0;&#xA0; return </span><span class="default">$a </span><span class="keyword">+ </span><span class="default">$b</span><span class="keyword">;<br>}<br><br></span><span class="default">$func </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br></span><span class="default">$values </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);<br></span><span class="default">call_user_func_array</span><span class="keyword">(</span><span class="default">$func</span><span class="keyword">, </span><span class="default">$values</span><span class="keyword">); <br></span><span class="comment">//returns 3<br><br></span><span class="default">$func</span><span class="keyword">(...</span><span class="default">$values</span><span class="keyword">);<br></span><span class="comment">//returns 3<br></span><span class="default">?&gt;<br></span><br>Benchmarks from <a href="https://gist.github.com/nikic/6390366" rel="nofollow" target="_blank">https://gist.github.com/nikic/6390366</a><br>cufa&#xA0;&#xA0; with 0 args took 0.43453288078308<br>switch with 0 args took 0.24134302139282<br>unpack with 0 args took 0.12418699264526<br>cufa&#xA0;&#xA0; with 5 args took 0.73408579826355<br>switch with 5 args took 0.49595499038696<br>unpack with 5 args took 0.18640494346619<br>cufa&#xA0;&#xA0; with 100 args took 5.0327250957489<br>switch with 100 args took 5.291127204895<br>unpack with 100 args took 1.2362589836121</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just hope this note helps someone (I killed the whole day on issue).
<br>
<br>If you use something like this in PHP &lt; 5.3:
<br><span class="default">&lt;?php call_user_func_array</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;parent::func&apos;</span><span class="keyword">), </span><span class="default">$args</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>Such a script will cause segmentation fault in your webserver.
<br>
<br>In 5.3 you should write it:
<br><span class="default">&lt;?php call_user_func_array</span><span class="keyword">(</span><span class="string">&apos;parent::func&apos;</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">); </span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.call-user-func-array.php)

**[⬆ to root](/)**