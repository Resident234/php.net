# mysqli::$affected_rows





On &quot;INSERT INTO ON DUPLICATE KEY UPDATE&quot; queries, though one may expect affected_rows to return only 0 or 1 per row on successful queries, it may in fact return 2.

From Mysql manual: &quot;With ON DUPLICATE KEY UPDATE, the affected-rows value per row is 1 if the row is inserted as a new row and 2 if an existing row is updated.&quot;

See: http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html

Here&apos;s the sum breakdown _per row_:
+0: a row wasn&apos;t updated or inserted (likely because the row already existed, but no field values were actually changed during the UPDATE)
+1: a row was inserted
+2: a row was updated

  

#



If you need to know specifically whether the WHERE condition of an UPDATE operation failed to match rows, or that simply no rows required updating you need to instead check mysqli::$info.

As this returns a string that requires parsing, you can use the following to convert the results into an associative array.

Object oriented style:



```
<?php
&#xA0; &#xA0; preg_match_all (&apos;/(\S[^:]+): (\d+)/&apos;, $mysqli-&gt;info, $matches); 
&#xA0; &#xA0; $info = array_combine ($matches[1], $matches[2]);
?>
```


Procedural style:



```
<?php
&#xA0; &#xA0; preg_match_all (&apos;/(\S[^:]+): (\d+)/&apos;, mysqli_info ($link), $matches); 
&#xA0; &#xA0; $info = array_combine ($matches[1], $matches[2]);
?>
```


You can then use the array to test for the different conditions



```
<?php
&#xA0; &#xA0; if ($info [&apos;Rows matched&apos;] == 0) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;This operation did not match any rows.\n&quot;;
&#xA0; &#xA0; } elseif ($info [&apos;Changed&apos;] == 0) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;This operation matched rows, but none required updating.\n&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; if ($info [&apos;Changed&apos;] &lt; $info [&apos;Rows matched&apos;]) {
&#xA0; &#xA0; &#xA0; &#xA0; echo ($info [&apos;Rows matched&apos;] - $info [&apos;Changed&apos;]).&quot; rows matched but were not changed.\n&quot;;
&#xA0; &#xA0; }
?>
```


This approach can be used with any query that mysqli::$info supports (INSERT INTO, LOAD DATA, ALTER TABLE, and UPDATE), for other any queries it returns an empty array.

For any UPDATE operation the array returned will have the following elements:

Array
(
&#xA0; &#xA0; [Rows matched] =&gt; 1
&#xA0; &#xA0; [Changed] =&gt; 0
&#xA0; &#xA0; [Warnings] =&gt; 0
)

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.affected-rows.php)

**[To root](/README.md)**