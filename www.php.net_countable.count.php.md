# Countable::count




<div class="phpcode"><span class="html">
Even though Countable::count method is called when the object implementing Countable is used in count() function, the second parameter of count, $mode, has no influence to your class method. <br><br>$mode is not passed to&#xA0; Countable::count:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">implements </span><span class="default">Countable<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">count</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">func_get_args</span><span class="keyword">());<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">count</span><span class="keyword">(new </span><span class="default">Foo</span><span class="keyword">(), </span><span class="default">COUNT_RECURSIVE</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>var_dump output:<br><br>array(0) {<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/countable.count.php)

**[To root](/README.md)**