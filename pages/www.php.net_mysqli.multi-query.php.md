# mysqli::multi_query



WATCH OUT: if you mix $mysqli-&gt;multi_query and $mysqli-&gt;query, the latter(s) won&apos;t be executed!<br><br>

```
<?php
// BAD CODE:
$mysqli->multi_query(" Many SQL queries ; "); // OK
$mysqli->query(" SQL statement #1 ; ") // not executed!
$mysqli->query(" SQL statement #2 ; ") // not executed!
$mysqli->query(" SQL statement #3 ; ") // not executed!
$mysqli->query(" SQL statement #4 ; ") // not executed!
?>
```


The only way to do this correctly is:



```
<?php
// WORKING CODE:
$mysqli->multi_query(" Many SQL queries ; "); // OK
while ($mysqli->next_result()) {;} // flush multi_queries
$mysqli->query(" SQL statement #1 ; ") // now executed!
$mysqli->query(" SQL statement #2 ; ") // now executed!
$mysqli->query(" SQL statement #3 ; ") // now executed!
$mysqli->query(" SQL statement #4 ; ") // now executed!
?>
```
  

---

To be able to execute a $mysqli-&gt;query() after a $mysqli-&gt;multi_query() for MySQL &gt; 5.3, I updated the code of jcn50 by this one :<br><br>

```
<?php
    // WORKING CODE:
    $mysqli->multi_query(" Many SQL queries ; "); // OK

    while ($mysqli->next_result()) // flush multi_queries
    {
        if (!$mysqli->more_results()) break;
    }

    $mysqli->query(" SQL statement #1 ; ") // now executed!
    $mysqli->query(" SQL statement #2 ; ") // now executed!
    $mysqli->query(" SQL statement #3 ; ") // now executed!
    $mysqli->query(" SQL statement #4 ; ") // now executed!
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mysqli.multi-query.php)

**[To root](/README.md)**