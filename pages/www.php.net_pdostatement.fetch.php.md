# PDOStatement::fetch





WARNING:
fetch() does NOT adhere to SQL-92 SQLSTATE standard when dealing with empty datasets.

Instead of setting the errorcode class to 20 to indicate &quot;no data found&quot;, it returns a class of 00 indicating success, and returns NULL to the caller.

This also prevents the exception mechainsm from firing.

Programmers will need to explicitly code tests for empty resultsets after any fetch*() instead of relying on the default behavior of the RDBMS.

I tried logging this as a bug, but it was dismissed as &quot;working as intended&quot;. Just a head&apos;s up.

  

#



If no record, this function will also return false.
I think that is not very good...

  

#



Someone&apos;s already pointed out that PDO::CURSOR_SCROLL isn&apos;t supported by the SQLite driver. It&apos;s also worth noting that it&apos;s not supported by the MySQL driver either.

In fact, if you try to use scrollable cursors with a MySQL statement, the PDO::FETCH_ORI_ABS parameter and the offset given to fetch() will be silently ignored. fetch() will behave as normal, returning rows in the order in which they came out of the database.

It&apos;s actually pretty confusing behaviour at first. Definitely worth documenting even if only as a user-added note on this page.

  

#



When using PDO::FETCH_COLUMN in a while loop, it&apos;s not enough to just use the value in the while statement as many examples show:



```
<?php
while ($row = $stmt-&gt;fetch(PDO::FETCH_COLUMN)) {
&#xA0; &#xA0; print $row;
}
?>
```


If there are 5 rows with values 1 2 0 4 5, then the while loop above will stop at the third row printing only 1 2. The solution is to either explicitly test for false:



```
<?php
while (($row = $stmt-&gt;fetch(PDO::FETCH_COLUMN)) !== false) {
&#xA0; &#xA0; print $row;
}
?>
```


Or use foreach with fetchAll():



```
<?php
foreach ($stmt-&gt;fetchAll(PDO::FETCH_COLUMN) as $row) {
&#xA0; &#xA0; print $row;
}
?>
```


Both will correctly print 1 2 0 4 5.

  

#



When fetching an object, the constructor of the class is called after the fields are populated by default.



PDO::FETCH_PROPS_LATE is used to change the behaviour and make it work as expected - constructor be called _before_ the object fields will be populated with the data.



sample:





```
<?php

$a = $PDO-&gt;query(&apos;select id from table&apos;);

$a-&gt;setFetchMode(PDO::FETCH_CLASS|PDO::FETCH_PROPS_LATE, &apos;ClassName&apos;);

$obj = $a-&gt;fetch();

?>
```




http://bugs.php.net/bug.php?id=53394

  

#



A quick one liner to get the first entry returned.&#xA0; This is nice for very basic queries.



```
<?php
$count = current($db-&gt;query(&quot;select count(*) from table&quot;)-&gt;fetch());
?>
```
php

  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetch.php)

**[To root](/README.md)**