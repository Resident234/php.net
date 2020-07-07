# Microsoft SQL Server





Hi, i was need some short and simple script to list all tables and columns of MSSQL database. There was nothing easy to explain on the net, so i&apos;ve decided to share my short script, i hope it will help.





```
<?php

$all = MSSQL_Query(&quot;select TABLE_NAME, COLUMN_NAME from INFORMATION_SCHEMA.COLUMNS order by TABLE_NAME, ORDINAL_POSITION&quot;);



$tables = array();

$columns = array();



while($fet_tbl = MSSQL_Fetch_Assoc($all)) { // PUSH ALL TABLES AND COLUMNS INTO THE ARRAY



&#xA0; $tables[] = $fet_tbl[TABLE_NAME];

&#xA0; $columns[] = $fet_tbl[COLUMN_NAME];



}



$sltml = array_count_values($tables); // HOW MANY COLUMNS ARE IN THE TABLE



foreach($sltml as $table_name =&gt; $id) {

 

 echo &quot;&lt;h2&gt;&quot;. $table_name .&quot; (&quot;. $id .&quot;)&lt;/h2&gt;&lt;ol&gt;&quot;;

 

&#xA0; &#xA0; for($i = 0; $i &lt;= $id-1; $i++) {

&#xA0; &#xA0; 

&#xA0; &#xA0; echo &quot;&lt;li&gt;&quot;. $columns[$i] .&quot;&lt;/li&gt;&quot;;

&#xA0; &#xA0; 

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; echo&quot;&lt;/ol&gt;&quot;;

 

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/book.mssql.php)

**[To root](/README.md)**