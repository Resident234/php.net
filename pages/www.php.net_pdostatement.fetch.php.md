# PDOStatement::fetch



WARNING:<br>fetch() does NOT adhere to SQL-92 SQLSTATE standard when dealing with empty datasets.<br><br>Instead of setting the errorcode class to 20 to indicate "no data found", it returns a class of 00 indicating success, and returns NULL to the caller.<br><br>This also prevents the exception mechainsm from firing.<br><br>Programmers will need to explicitly code tests for empty resultsets after any fetch*() instead of relying on the default behavior of the RDBMS.<br><br>I tried logging this as a bug, but it was dismissed as "working as intended". Just a head&apos;s up.  

#

If no record, this function will also return false.<br>I think that is not very good...  

#

Someone&apos;s already pointed out that PDO::CURSOR_SCROLL isn&apos;t supported by the SQLite driver. It&apos;s also worth noting that it&apos;s not supported by the MySQL driver either.<br><br>In fact, if you try to use scrollable cursors with a MySQL statement, the PDO::FETCH_ORI_ABS parameter and the offset given to fetch() will be silently ignored. fetch() will behave as normal, returning rows in the order in which they came out of the database.<br><br>It&apos;s actually pretty confusing behaviour at first. Definitely worth documenting even if only as a user-added note on this page.  

#

When using PDO::FETCH_COLUMN in a while loop, it&apos;s not enough to just use the value in the while statement as many examples show:<br><br>

```
<?php
while ($row = $stmt->fetch(PDO::FETCH_COLUMN)) {
    print $row;
}
?>
```


If there are 5 rows with values 1 2 0 4 5, then the while loop above will stop at the third row printing only 1 2. The solution is to either explicitly test for false:



```
<?php
while (($row = $stmt->fetch(PDO::FETCH_COLUMN)) !== false) {
    print $row;
}
?>
```


Or use foreach with fetchAll():



```
<?php
foreach ($stmt->fetchAll(PDO::FETCH_COLUMN) as $row) {
    print $row;
}
?>
```
<br><br>Both will correctly print 1 2 0 4 5.  

#

When fetching an object, the constructor of the class is called after the fields are populated by default.<br><br>PDO::FETCH_PROPS_LATE is used to change the behaviour and make it work as expected - constructor be called _before_ the object fields will be populated with the data.<br><br>sample:<br><br>

```
<?php
$a = $PDO->query('select id from table');
$a->setFetchMode(PDO::FETCH_CLASS|PDO::FETCH_PROPS_LATE, 'ClassName');
$obj = $a->fetch();
?>
```
<br><br>http://bugs.php.net/bug.php?id=53394  

#

A quick one liner to get the first entry returned.  This is nice for very basic queries.<br><br>

```
<?php
$count = current($db->query("select count(*) from table")->fetch());
?>
```
php  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetch.php)

**[To root](/README.md)**