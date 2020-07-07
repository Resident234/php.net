# mysqli::rollback





Remember that MyISAM tables do not support rollbacks.

I just drove myself crazy for an afternoon trying to figure out what was wrong with my code - meanwhile it was fine all along

  

#



This is an example to explain the powerful of the rollback and commit functions.

Let&apos;s suppose you want to be sure that all queries have to be executed without errors before writing data on the database.

Here&apos;s the code:





```
<?php

$all_query_ok=true; // our control variable



//we make 4 inserts, the last one generates an error

//if at least one query returns an error we change our control variable

$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (100)&quot;) ? null : $all_query_ok=false;

$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (200)&quot;) ? null : $all_query_ok=false;

$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (300)&quot;) ? null : $all_query_ok=false;

$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (100)&quot;) ? null : $all_query_ok=false; //duplicated PRIMARY KEY VALUE



//now let&apos;s test our control variable

$all_query_ok ? $mysqli-&gt;commit() : $mysqli-&gt;rollback();



$mysqli-&gt;close();

?>
```




hope to be helpful!

  

#



Just a note about auto incremental ids and rollback.
When using transactions and inserting into a table containing a column with auto incremental ids, the id will be incremented even though the transaction is rolled back.

This might occupy a lot of ids if a lot of rollbacks are performed.

Example:


```
<?php
$mysqli = new mysqli(&quot;localhost&quot;, &quot;gugbageri&quot;, &quot;gugbageri&quot;, &quot;gugbageri&quot;);

/* check connection */
if (mysqli_connect_errno()) {
&#xA0; &#xA0; printf(&quot;Connect failed: %s\n&quot;, mysqli_connect_error());
&#xA0; &#xA0; exit();
}

/* disable autocommit */
$mysqli-&gt;autocommit(FALSE);

/* We just create a test table with one auto incremental primary column and a content column*/
$mysqli-&gt;query(&quot;CREATE TABLE TestTable ( `id_column` INT NOT NULL&#xA0; AUTO_INCREMENT , `content` INT NOT NULL , PRIMARY KEY ( `id_column` )) ENGINE = InnoDB;&quot;);

/* commit newly created table */
$mysqli-&gt;commit();

/* we insert a row */
$mysqli-&gt;query(&quot;INSERT INTO TestTable (content) VALUES (99)&quot;);

/* we commit the inserted row */
$mysqli-&gt;commit();

/* we insert another three rows */
$mysqli-&gt;query(&quot;INSERT INTO TestTable (content) VALUES (99)&quot;);
$mysqli-&gt;query(&quot;INSERT INTO TestTable (content) VALUES (99)&quot;);
$mysqli-&gt;query(&quot;INSERT INTO TestTable (content) VALUES (99)&quot;);

/* we the rollback */
$mysqli-&gt;rollback();

/* we insert a row */
$mysqli-&gt;query(&quot;INSERT INTO TestTable (content) VALUES (99)&quot;);

/* we commit the inserted row */
$mysqli-&gt;commit();

if ($result = $mysqli-&gt;query(&quot;SELECT id_column FROM TestTable&quot;)) {

&#xA0;&#xA0; while($row = $result-&gt;fetch_row()) {
&#xA0; &#xA0; &#xA0; printf(&quot;Id: %d.\n&quot;, $row[0]);
&#xA0;&#xA0; }
&#xA0; &#xA0; /* Free result */
&#xA0; &#xA0; $result-&gt;close();
}

/* Drop table TestTable */
$mysqli-&gt;query(&quot;DROP TABLE TestTable&quot;);

$mysqli-&gt;close();
?>
```


This will output:
Id: 1.
Id: 5.

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.rollback.php)

**[To root](/README.md)**