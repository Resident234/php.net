# PDOStatement::fetchColumn



fetchColumn return boolean false when a row not is found or don&apos;t had more rows.  

#

This is an excellent method for returning a column count. For example:<br><br>

```
<?php
$db = new PDO(&apos;mysql:host=localhost;dbname=pictures&apos;,&apos;user&apos;,&apos;password&apos;);
$pics = $db-&gt;query(&apos;SELECT COUNT(id) FROM pics&apos;);
$this-&gt;totalpics = $pics-&gt;fetchColumn();
$db = null;
?>
```
<br>In my case $pics-&gt;fetchColumn() returns 641 because that is how many pictures I have in my db.  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchcolumn.php)

**[To root](/README.md)**