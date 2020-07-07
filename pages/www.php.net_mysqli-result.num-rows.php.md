# mysqli_result::$num_rows





If you have problems making work this num_rows, you have to declare -&gt;store_result() first.



```
<?php
$mysqli = new mysqli(&quot;localhost&quot;,&quot;root&quot;, &quot;&quot;, &quot;tables&quot;);

$query = $mysqli-&gt;prepare(&quot;SELECT * FROM table1&quot;);
$query-&gt;execute();
$query-&gt;store_result();

$rows = $query-&gt;num_rows;

echo $rows;

// Return 4 for example
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.num-rows.php)

**[To root](/README.md)**