# odbc_prepare




<div class="phpcode"><span class="html">
Is it just me or is the code above misleading? It makes it look like odbc_execute() returns a resource suitable, say, for passing to one of the odbc_fetch_* functions.<br><br>In fact, odbc_execute() returns a boolean, which simply indicates success (TRUE) or failure (FALSE). The variable to pass to odbc_fetch_* is the same one that you pass to odbc_execute():<br><br><span class="default">&lt;?php<br>$res </span><span class="keyword">= </span><span class="default">odbc_prepare</span><span class="keyword">(</span><span class="default">$db_conn</span><span class="keyword">, </span><span class="default">$query_string</span><span class="keyword">);<br>if(!</span><span class="default">$res</span><span class="keyword">) die(</span><span class="string">&quot;could not prepare statement &quot;</span><span class="keyword">.</span><span class="default">$query_string</span><span class="keyword">);<br><br>if(</span><span class="default">odbc_execute</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">, </span><span class="default">$parameters</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="default">$row </span><span class="keyword">= </span><span class="default">odbc_fetch_array</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">);<br>} else {<br>&#xA0; &#xA0; </span><span class="comment">// handle error<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.odbc-prepare.php)

**[To root](/README.md)**