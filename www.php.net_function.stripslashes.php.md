# stripslashes




<div class="phpcode"><span class="html">
Sometimes for some reason is happens that PHP or Javascript or some naughty insert a lot of&#xA0; backslash. Ordinary function does not notice that. Therefore, it is necessary that the bit &quot;inflate&quot;:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">removeslashes</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$string</span><span class="keyword">=</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;\\&quot;</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">));<br>&#xA0; &#xA0; return </span><span class="default">stripslashes</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">));<br>}<br><br></span><span class="comment">/* Example */<br><br></span><span class="default">$text</span><span class="keyword">=</span><span class="string">&quot;My dog don\\\\\\\\\\\\\\\\&apos;t like the postman!&quot;</span><span class="keyword">;<br>echo </span><span class="default">removeslashes</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>RESULT: My dog don&apos;t like the postman!<br><br>This flick has served me wery well, because I had this problem before.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stripslashes.php)

**[To root](/README.md)**