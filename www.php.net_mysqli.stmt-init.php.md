# mysqli::stmt_initmysqli_stmt_init




<div class="phpcode"><span class="html">
stmt_init() seems to clear previous (possibly erroneous) results on the DB connection, which means you don&apos;t necessarily need to use it but it could make the code more robust.<br><br>In a PHPUnit test, I had a sequence of prepared queries on the same connection. One of them fetched a row from a SELECT but didn&apos;t keep fetching until it drained the connection, so it left some stale results. When the next query did this:<br><br><span class="default">&lt;?php<br>$db </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">getConnection</span><span class="keyword">()-&gt;</span><span class="default">getDbConnection</span><span class="keyword">();<br></span><span class="default">$preparedQuery </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare </span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>the prepare() call generated an error: &quot;Could not prepare query: Commands out of sync; you can&apos;t run this command now.&quot; Changing to this:<br><br><span class="default">&lt;?php<br>$db </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">getConnection</span><span class="keyword">()-&gt;</span><span class="default">getDbConnection</span><span class="keyword">();<br></span><span class="default">$preparedQuery </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">stmt_init</span><span class="keyword">();<br></span><span class="default">$preparedQuery</span><span class="keyword">-&gt;</span><span class="default">prepare </span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>resolved the problem.</span>
</div>
  

#


<div class="phpcode"><span class="html">
you can use $stmt = $mysqli-&gt;prepare(); directly without stmt-init() . i think there is no need for stmt-init .</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.stmt-init.php)

**[To root](/README.md)**