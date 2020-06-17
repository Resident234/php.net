# SQLite3Stmt::bindValue




<div class="phpcode"><span class="html">
Note that this also works with positional placeholders using the &apos;?&apos; token:<br><br><span class="default">&lt;?php<br><br>$stmt </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT * FROM mytable WHERE foo = ? AND bar = ?&apos;</span><span class="keyword">);<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">bindValue</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;somestring&apos;</span><span class="keyword">, </span><span class="default">SQLITE3_TEXT</span><span class="keyword">);<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">bindValue</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">, </span><span class="default">42</span><span class="keyword">, </span><span class="default">SQLITE3_INTEGER</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Positional numbering starts at 1.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3stmt.bindvalue.php)

**[To root](/README.md)**