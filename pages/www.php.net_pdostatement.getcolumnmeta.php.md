# PDOStatement::getColumnMeta




<div class="phpcode"><span class="html">
This method is supported in the MySQL 5.0+ driver.&#xA0; It can be used for object hydration:<br><br><span class="default">&lt;?php<br><br>$pdo_stmt </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="string">&apos;SELECT discussion.id, discussion.text, comment.id, comment.text FROM discussions LEFT JOIN comments ON comment.discussion_id = discussion.id&apos;</span><span class="keyword">);<br><br>foreach(</span><span class="default">range</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">$pdo_stmt</span><span class="keyword">-&gt;</span><span class="default">columnCount</span><span class="keyword">() - </span><span class="default">1</span><span class="keyword">) as </span><span class="default">$column_index</span><span class="keyword">)<br>{<br>&#xA0; </span><span class="default">$meta</span><span class="keyword">[] = </span><span class="default">$pdo_stmt</span><span class="keyword">-&gt;</span><span class="default">getColumnMeta</span><span class="keyword">(</span><span class="default">$column_index</span><span class="keyword">);<br>}<br><br>while(</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$pdo_stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_NUM</span><span class="keyword">))<br>{<br>&#xA0; foreach(</span><span class="default">$row </span><span class="keyword">as </span><span class="default">$column_index </span><span class="keyword">=&gt; </span><span class="default">$column_value</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; </span><span class="comment">//do something with the data, using the ids to establish the discussion.has_many(comments) relationship.<br>&#xA0; </span><span class="keyword">}<br>}<br><br></span><span class="default">?&gt;<br></span><br>If you are building an ORM, this method is very useful to support more natural SQL syntax.&#xA0; Most ORMs require the column names to be aliases so that they can be parsed and turned into objects that properly represent has_one, has_many, many_to_many relationships.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.getcolumnmeta.php)

**[To root](/README.md)**