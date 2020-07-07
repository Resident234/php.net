# mysqli::multi_query





WATCH OUT: if you mix $mysqli-&gt;multi_query and $mysqli-&gt;query, the latter(s) won&apos;t be executed!



```
<?php
// BAD CODE:
$mysqli-&gt;multi_query(&quot; Many SQL queries ; &quot;); // OK
$mysqli-&gt;query(&quot; SQL statement #1 ; &quot;) // not executed!
$mysqli-&gt;query(&quot; SQL statement #2 ; &quot;) // not executed!
$mysqli-&gt;query(&quot; SQL statement #3 ; &quot;) // not executed!
$mysqli-&gt;query(&quot; SQL statement #4 ; &quot;) // not executed!
?>
```


The only way to do this correctly is:



```
<?php
// WORKING CODE:
$mysqli-&gt;multi_query(&quot; Many SQL queries ; &quot;); // OK
while ($mysqli-&gt;next_result()) {;} // flush multi_queries
$mysqli-&gt;query(&quot; SQL statement #1 ; &quot;) // now executed!
$mysqli-&gt;query(&quot; SQL statement #2 ; &quot;) // now executed!
$mysqli-&gt;query(&quot; SQL statement #3 ; &quot;) // now executed!
$mysqli-&gt;query(&quot; SQL statement #4 ; &quot;) // now executed!
?>
```



  

#



To be able to execute a $mysqli-&gt;query() after a $mysqli-&gt;multi_query() for MySQL &gt; 5.3, I updated the code of jcn50 by this one :



```
<?php
&#xA0; &#xA0; // WORKING CODE:
&#xA0; &#xA0; $mysqli-&gt;multi_query(&quot; Many SQL queries ; &quot;); // OK

&#xA0; &#xA0; while ($mysqli-&gt;next_result()) // flush multi_queries
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!$mysqli-&gt;more_results()) break;
&#xA0; &#xA0; }

&#xA0; &#xA0; $mysqli-&gt;query(&quot; SQL statement #1 ; &quot;) // now executed!
&#xA0; &#xA0; $mysqli-&gt;query(&quot; SQL statement #2 ; &quot;) // now executed!
&#xA0; &#xA0; $mysqli-&gt;query(&quot; SQL statement #3 ; &quot;) // now executed!
&#xA0; &#xA0; $mysqli-&gt;query(&quot; SQL statement #4 ; &quot;) // now executed!
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.multi-query.php)

**[To root](/README.md)**