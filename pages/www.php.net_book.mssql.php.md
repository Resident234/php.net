# Microsoft SQL Server



Hi, i was need some short and simple script to list all tables and columns of MSSQL database. There was nothing easy to explain on the net, so i&apos;ve decided to share my short script, i hope it will help.<br><br>

```
<?php
$all = MSSQL_Query("select TABLE_NAME, COLUMN_NAME from INFORMATION_SCHEMA.COLUMNS order by TABLE_NAME, ORDINAL_POSITION");

$tables = array();
$columns = array();

while($fet_tbl = MSSQL_Fetch_Assoc($all)) { // PUSH ALL TABLES AND COLUMNS INTO THE ARRAY

  $tables[] = $fet_tbl[TABLE_NAME];
  $columns[] = $fet_tbl[COLUMN_NAME];

}

$sltml = array_count_values($tables); // HOW MANY COLUMNS ARE IN THE TABLE

foreach($sltml as $table_name => $id) {
 
 echo "<h2>". $table_name ." (". $id .")</h2><ol>";
 
    for($i = 0; $i <= $id-1; $i++) {
    
    echo "<li>". $columns[$i] ."</li>";
    
    }
    
  echo"</ol>";
 
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/book.mssql.php)

**[To root](/README.md)**