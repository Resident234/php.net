# The SQLite3Result class




<div class="phpcode"><span class="html">
Since SQLite3Result::numRows is unavailable, use:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if (</span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">numColumns</span><span class="keyword">() &amp;&amp; </span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">columnType</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">) != </span><span class="default">SQLITE3_NULL</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">// have rows
<br></span><span class="keyword">} else {
<br>&#xA0; &#xA0; </span><span class="comment">// zero rows
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>
<br>Because when there are zero rows:
<br>* SQLite3Result::fetchArray will return &apos;1&apos;
<br>* SQLite3Result::numColumns will return &apos;1&apos;
<br>* Column type for column &apos;0&apos; will be SQLITE3_NULL</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.sqlite3result.php)

**[To root](/README.md)**