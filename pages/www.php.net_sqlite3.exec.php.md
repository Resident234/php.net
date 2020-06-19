# SQLite3::exec




<div class="phpcode"><span class="html">
I was getting &quot;database locked&quot; all the time until I found out some features of sqlite3 must be set by using SQL special instructions (i.e. using PRAGMA keyword). For instance, what apparently solved my problem with &quot;database locked&quot; was to set journal_mode to &apos;wal&apos; (it is defaulting to &apos;delete&apos;, as stated here: <a href="https://www.sqlite.org/wal.html" rel="nofollow" target="_blank">https://www.sqlite.org/wal.html</a> (see Activating&#xA0; And Configuring WAL Mode)).<br><br>So basically what I had to do was creating a connection to the database and setting journal_mode with the SQL statement. Example:<br><br><span class="default">&lt;?php<br>$db </span><span class="keyword">= new </span><span class="default">SQLite3</span><span class="keyword">(</span><span class="string">&apos;/my/sqlite/file.sqlite3&apos;</span><span class="keyword">);<br></span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">busyTimeout</span><span class="keyword">(</span><span class="default">5000</span><span class="keyword">);<br></span><span class="comment">// WAL mode has better control over concurrency.<br>// Source: <a href="https://www.sqlite.org/wal.html" rel="nofollow" target="_blank">https://www.sqlite.org/wal.html</a><br></span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">exec</span><span class="keyword">(</span><span class="string">&apos;PRAGMA journal_mode = wal;&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Hope that helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3.exec.php)

**[To root](/README.md)**