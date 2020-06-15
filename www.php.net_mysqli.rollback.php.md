# mysqli::rollbackmysqli_rollback




<div class="phpcode"><span class="html">
Remember that MyISAM tables do not support rollbacks.<br><br>I just drove myself crazy for an afternoon trying to figure out what was wrong with my code - meanwhile it was fine all along</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is an example to explain the powerful of the rollback and commit functions.
<br>Let&apos;s suppose you want to be sure that all queries have to be executed without errors before writing data on the database.
<br>Here&apos;s the code:
<br>
<br><span class="default">&lt;?php
<br>$all_query_ok</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">; </span><span class="comment">// our control variable
<br>
<br>//we make 4 inserts, the last one generates an error
<br>//if at least one query returns an error we change our control variable
<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (100)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;
<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (200)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;
<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (300)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;
<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (100)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">; </span><span class="comment">//duplicated PRIMARY KEY VALUE
<br>
<br>//now let&apos;s test our control variable
<br></span><span class="default">$all_query_ok </span><span class="keyword">? </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">() : </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">rollback</span><span class="keyword">();
<br>
<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>hope to be helpful!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a note about auto incremental ids and rollback.<br>When using transactions and inserting into a table containing a column with auto incremental ids, the id will be incremented even though the transaction is rolled back.<br><br>This might occupy a lot of ids if a lot of rollbacks are performed.<br><br>Example:<br><span class="default">&lt;?php<br>$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">, </span><span class="string">&quot;gugbageri&quot;</span><span class="keyword">, </span><span class="string">&quot;gugbageri&quot;</span><span class="keyword">, </span><span class="string">&quot;gugbageri&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* check connection */<br></span><span class="keyword">if (</span><span class="default">mysqli_connect_errno</span><span class="keyword">()) {<br>&#xA0; &#xA0; </span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;Connect failed: %s\n&quot;</span><span class="keyword">, </span><span class="default">mysqli_connect_error</span><span class="keyword">());<br>&#xA0; &#xA0; exit();<br>}<br><br></span><span class="comment">/* disable autocommit */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">autocommit</span><span class="keyword">(</span><span class="default">FALSE</span><span class="keyword">);<br><br></span><span class="comment">/* We just create a test table with one auto incremental primary column and a content column*/<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;CREATE TABLE TestTable ( `id_column` INT NOT NULL&#xA0; AUTO_INCREMENT , `content` INT NOT NULL , PRIMARY KEY ( `id_column` )) ENGINE = InnoDB;&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* commit newly created table */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">();<br><br></span><span class="comment">/* we insert a row */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO TestTable (content) VALUES (99)&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* we commit the inserted row */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">();<br><br></span><span class="comment">/* we insert another three rows */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO TestTable (content) VALUES (99)&quot;</span><span class="keyword">);<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO TestTable (content) VALUES (99)&quot;</span><span class="keyword">);<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO TestTable (content) VALUES (99)&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* we the rollback */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">rollback</span><span class="keyword">();<br><br></span><span class="comment">/* we insert a row */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO TestTable (content) VALUES (99)&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* we commit the inserted row */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">();<br><br>if (</span><span class="default">$result </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;SELECT id_column FROM TestTable&quot;</span><span class="keyword">)) {<br><br>&#xA0;&#xA0; while(</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_row</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;Id: %d.\n&quot;</span><span class="keyword">, </span><span class="default">$row</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);<br>&#xA0;&#xA0; }<br>&#xA0; &#xA0; </span><span class="comment">/* Free result */<br>&#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>}<br><br></span><span class="comment">/* Drop table TestTable */<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;DROP TABLE TestTable&quot;</span><span class="keyword">);<br><br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>This will output:<br>Id: 1.<br>Id: 5.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.rollback.php)

**[To root](/README.md)**