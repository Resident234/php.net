# mysqli::rollback



Remember that MyISAM tables do not support rollbacks.<br><br>I just drove myself crazy for an afternoon trying to figure out what was wrong with my code - meanwhile it was fine all along  

#

This is an example to explain the powerful of the rollback and commit functions.<br>Let&apos;s suppose you want to be sure that all queries have to be executed without errors before writing data on the database.<br>Here&apos;s the code:<br><br>

```
<?php
$all_query_ok=true; // our control variable

//we make 4 inserts, the last one generates an error
//if at least one query returns an error we change our control variable
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (100)") ? null : $all_query_ok=false;
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (200)") ? null : $all_query_ok=false;
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (300)") ? null : $all_query_ok=false;
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (100)") ? null : $all_query_ok=false; //duplicated PRIMARY KEY VALUE

//now let&apos;s test our control variable
$all_query_ok ? $mysqli-&gt;commit() : $mysqli-&gt;rollback();

$mysqli-&gt;close();
?>
```
<br><br>hope to be helpful!  

#

Just a note about auto incremental ids and rollback.<br>When using transactions and inserting into a table containing a column with auto incremental ids, the id will be incremented even though the transaction is rolled back.<br><br>This might occupy a lot of ids if a lot of rollbacks are performed.<br><br>Example:<br>

```
<?php
$mysqli = new mysqli("localhost", "gugbageri", "gugbageri", "gugbageri");

/* check connection */
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

/* disable autocommit */
$mysqli-&gt;autocommit(FALSE);

/* We just create a test table with one auto incremental primary column and a content column*/
$mysqli-&gt;query("CREATE TABLE TestTable ( `id_column` INT NOT NULL  AUTO_INCREMENT , `content` INT NOT NULL , PRIMARY KEY ( `id_column` )) ENGINE = InnoDB;");

/* commit newly created table */
$mysqli-&gt;commit();

/* we insert a row */
$mysqli-&gt;query("INSERT INTO TestTable (content) VALUES (99)");

/* we commit the inserted row */
$mysqli-&gt;commit();

/* we insert another three rows */
$mysqli-&gt;query("INSERT INTO TestTable (content) VALUES (99)");
$mysqli-&gt;query("INSERT INTO TestTable (content) VALUES (99)");
$mysqli-&gt;query("INSERT INTO TestTable (content) VALUES (99)");

/* we the rollback */
$mysqli-&gt;rollback();

/* we insert a row */
$mysqli-&gt;query("INSERT INTO TestTable (content) VALUES (99)");

/* we commit the inserted row */
$mysqli-&gt;commit();

if ($result = $mysqli-&gt;query("SELECT id_column FROM TestTable")) {

   while($row = $result-&gt;fetch_row()) {
      printf("Id: %d.\n", $row[0]);
   }
    /* Free result */
    $result-&gt;close();
}

/* Drop table TestTable */
$mysqli-&gt;query("DROP TABLE TestTable");

$mysqli-&gt;close();
?>
```
<br><br>This will output:<br>Id: 1.<br>Id: 5.  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.rollback.php)

**[To root](/README.md)**