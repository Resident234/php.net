# mysqli_result::fetch_array




<div class="phpcode"><span class="html">
Putting multiple rows into an array:<br><br><span class="default">&lt;?php<br>$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">, </span><span class="string">&quot;my_user&quot;</span><span class="keyword">, </span><span class="string">&quot;my_password&quot;</span><span class="keyword">, </span><span class="string">&quot;world&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* check connection */<br></span><span class="keyword">if (</span><span class="default">mysqli_connect_errno</span><span class="keyword">()) {<br>&#xA0; &#xA0; </span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;Connect failed: %s\n&quot;</span><span class="keyword">, </span><span class="default">mysqli_connect_error</span><span class="keyword">());<br>&#xA0; &#xA0; exit();<br>}<br><br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;SELECT Name, CountryCode FROM City ORDER by ID LIMIT 3&quot;</span><span class="keyword">;<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">);<br><br>while(</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_array</span><span class="keyword">())<br>{<br></span><span class="default">$rows</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">;<br>}<br><br>foreach(</span><span class="default">$rows </span><span class="keyword">as </span><span class="default">$row</span><span class="keyword">)<br>{<br>echo </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;CountryCode&apos;</span><span class="keyword">];<br>}<br><br></span><span class="comment">/* free result set */<br></span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br><br></span><span class="comment">/* close connection */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-array.php)

**[⬆ to root](/)**