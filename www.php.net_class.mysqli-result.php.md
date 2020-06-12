# The mysqli_result class




<div class="phpcode"><span class="html">
Converting an old project from using the mysql extension to the mysqli extension, I found the most annoying change to be the lack of a corresponding mysql_result function in mysqli. While mysql_result is a generally terrible function, it was useful for fetching a single result field *value* from a result set (for example, if looking up a user&apos;s ID).
<br>
<br>The behavior of mysql_result is approximated here, though you may want to name it something other than mysqli_result so as to avoid thinking it&apos;s an actual, built-in function.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mysqli_result</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">, </span><span class="default">$row</span><span class="keyword">, </span><span class="default">$field</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">data_seek</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$datarow </span><span class="keyword">= </span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">fetch_array</span><span class="keyword">();
<br>&#xA0; &#xA0; return </span><span class="default">$datarow</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">];
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Implementing it via the OO interface is left as an exercise to the reader.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.mysqli-result.php)

**[⬆ to root](/)**