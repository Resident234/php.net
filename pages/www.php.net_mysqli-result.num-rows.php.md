# mysqli_result::$num_rows




<div class="phpcode"><span class="html">
If you have problems making work this num_rows, you have to declare -&gt;store_result() first.<br><br><span class="default">&lt;?php<br>$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">,</span><span class="string">&quot;root&quot;</span><span class="keyword">, </span><span class="string">&quot;&quot;</span><span class="keyword">, </span><span class="string">&quot;tables&quot;</span><span class="keyword">);<br><br></span><span class="default">$query </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;SELECT * FROM table1&quot;</span><span class="keyword">);<br></span><span class="default">$query</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br></span><span class="default">$query</span><span class="keyword">-&gt;</span><span class="default">store_result</span><span class="keyword">();<br><br></span><span class="default">$rows </span><span class="keyword">= </span><span class="default">$query</span><span class="keyword">-&gt;</span><span class="default">num_rows</span><span class="keyword">;<br><br>echo </span><span class="default">$rows</span><span class="keyword">;<br><br></span><span class="comment">// Return 4 for example<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.num-rows.php)

**[To root](/README.md)**