# SQLite3::query




<div class="phpcode"><span class="html">
The recommended way to do a SQLite3 query is to use a statement. For a table creation, a query might be fine (and easier) but for an insert, update or select, you should really use a statement, it&apos;s really easier and safer as SQLite will escape your parameters according to their type. SQLite will also use less memory than if you created the whole query by yourself. Example:<br><br><span class="default">&lt;?php<br><br>$db </span><span class="keyword">= new </span><span class="default">SQLite3</span><span class="keyword">;<br></span><span class="default">$statement </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT * FROM table WHERE id = :id;&apos;</span><span class="keyword">);<br></span><span class="default">$statement</span><span class="keyword">-&gt;</span><span class="default">bindValue</span><span class="keyword">(</span><span class="string">&apos;:id&apos;</span><span class="keyword">, </span><span class="default">$id</span><span class="keyword">);<br><br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$statement</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>You can also re-use a statement and change its parameters, just do $statement-&gt;reset(). Finally don&apos;t forget to close a statement when you don&apos;t need it anymore as it will free some memory.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3.query.php)

**[To root](/README.md)**