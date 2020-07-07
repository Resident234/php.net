# PDOStatement::rowCount





When updating a Mysql table with identical values nothing&apos;s really affected so rowCount will return 0. As Mr. Perl below noted this is not always preferred behaviour and you can change it yourself since PHP 5.3.

Just create your PDO object with 
&lt;? php
$p = new PDO($dsn, $u, $p, array(PDO::MYSQL_ATTR_FOUND_ROWS =&gt; true));
?>
```

and rowCount() will tell you how many rows your update-query actually found/matched.

  

#



Great, while using MySQL5, the only way to get the number of rows after doing a PDO SELECT query is to either execute a separate SELECT COUNT(*) query (or to do count($stmt-&gt;fetchAll()), which seems like a ridiculous waste of overhead and programming time.

Another gripe I have about PDO is its inability to get the value of output parameters from stored procedures in some DBMSs, such as SQL Server.

I&apos;m not so sure I&apos;m diggin&apos; PDO yet.

  

#



Note that an INSERT ... ON DUPLICATE KEY UPDATE statement is not an INSERT statement, rowCount won&apos;t return the number or rows inserted or updated for such a statement.&#xA0; For MySQL, it will return 1 if the row is inserted, and 2 if it is updated, but that may not apply to other databases.

  

#



To display information only when the query is not empty, I do something like this:



```
<?php
&#xA0; &#xA0; $sql = &apos;SELECT model FROM cars&apos;;
&#xA0; &#xA0; $stmt = $db-&gt;prepare($sql);
&#xA0; &#xA0; $stmt-&gt;execute();
&#xA0; &#xA0; 
&#xA0; &#xA0; if ($data = $stmt-&gt;fetch()) {
&#xA0; &#xA0; &#xA0; &#xA0; do {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo $data[&apos;model&apos;] . &apos;&lt;br&gt;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; } while ($data = $stmt-&gt;fetch());
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Empty Query&apos;;
&#xA0; &#xA0; }
?>
```



  

#



It&apos;d better to use SQL_CALC_FOUND_ROWS, if you only use MySQL. It has many advantages as you could retrieve only part of result set (via LIMIT) but still get the total row count.

code:



```
<?php

$db = new PDO(DSN...);

$db-&gt;setAttribute(array(PDO::MYSQL_USE_BUFFERED_QUERY=&gt;TRUE));

$rs&#xA0; = $db-&gt;query(&apos;SELECT SQL_CALC_FOUND_ROWS * FROM table LIMIT 5,15&apos;);

$rs1 = $db-&gt;query(&apos;SELECT FOUND_ROWS()&apos;);

$rowCount = (int) $rs1-&gt;fetchColumn();

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.rowcount.php)

**[To root](/README.md)**