# PDOStatement::rowCount




<div class="phpcode"><span class="html">
When updating a Mysql table with identical values nothing&apos;s really affected so rowCount will return 0. As Mr. Perl below noted this is not always preferred behaviour and you can change it yourself since PHP 5.3.<br><br>Just create your PDO object with <br>&lt;? php<br>$p = new PDO($dsn, $u, $p, array(PDO::MYSQL_ATTR_FOUND_ROWS =&gt; true));<br>?&gt;<br>and rowCount() will tell you how many rows your update-query actually found/matched.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Great, while using MySQL5, the only way to get the number of rows after doing a PDO SELECT query is to either execute a separate SELECT COUNT(*) query (or to do count($stmt-&gt;fetchAll()), which seems like a ridiculous waste of overhead and programming time.<br><br>Another gripe I have about PDO is its inability to get the value of output parameters from stored procedures in some DBMSs, such as SQL Server.<br><br>I&apos;m not so sure I&apos;m diggin&apos; PDO yet.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that an INSERT ... ON DUPLICATE KEY UPDATE statement is not an INSERT statement, rowCount won&apos;t return the number or rows inserted or updated for such a statement.&#xA0; For MySQL, it will return 1 if the row is inserted, and 2 if it is updated, but that may not apply to other databases.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To display information only when the query is not empty, I do something like this:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $sql </span><span class="keyword">= </span><span class="string">&apos;SELECT model FROM cars&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (</span><span class="default">$data </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; &#xA0; do {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$data</span><span class="keyword">[</span><span class="string">&apos;model&apos;</span><span class="keyword">] . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } while (</span><span class="default">$data </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">());<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Empty Query&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;d better to use SQL_CALC_FOUND_ROWS, if you only use MySQL. It has many advantages as you could retrieve only part of result set (via LIMIT) but still get the total row count.
<br>code:
<br><span class="default">&lt;?php
<br>$db </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="default">DSN</span><span class="keyword">...);
<br></span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(array(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">MYSQL_USE_BUFFERED_QUERY</span><span class="keyword">=&gt;</span><span class="default">TRUE</span><span class="keyword">));
<br></span><span class="default">$rs&#xA0; </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&apos;SELECT SQL_CALC_FOUND_ROWS * FROM table LIMIT 5,15&apos;</span><span class="keyword">);
<br></span><span class="default">$rs1 </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&apos;SELECT FOUND_ROWS()&apos;</span><span class="keyword">);
<br></span><span class="default">$rowCount </span><span class="keyword">= (int) </span><span class="default">$rs1</span><span class="keyword">-&gt;</span><span class="default">fetchColumn</span><span class="keyword">();
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.rowcount.php)

**[To root](/README.md)**