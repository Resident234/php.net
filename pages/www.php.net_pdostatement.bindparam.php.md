# PDOStatement::bindParam



I know this has been said before but I&apos;ll write a note on it too because I think it&apos;s important to keep in mind:<br><br>If you use PDO bindParam to do a search with a LIKE condition you cannot put the percentages and quotes to the param placeholder &apos;%:keyword%&apos;.<br><br>This is WRONG:<br>"SELECT * FROM `users` WHERE `firstname` LIKE &apos;%:keyword%&apos;";<br><br>The CORRECT solution is to leave clean the placeholder like this:<br>"SELECT * FROM `users` WHERE `firstname` LIKE :keyword";<br><br>And then add the percentages to the php variable where you store the keyword:<br>$keyword = "%".$keyword."%";<br><br>And finally the quotes will be automatically added by PDO when executing the query so you don&apos;t have to worry about them.<br><br>So the full example would be:<br>

```
<?php
// Get the keyword from query string
$keyword = $_GET['keyword'];
// Prepare the command
$sth = $dbh->prepare('SELECT * FROM `users` WHERE `firstname` LIKE :keyword');
// Put the percentage sing on the keyword
$keyword = "%".$keyword."%";
// Bind the parameter
$sth->bindParam(':keyword', $keyword, PDO::PARAM_STR);
?>
```
  

#

This works ($val by reference):<br>

```
<?php
foreach ($params as $key => &amp;$val) {
    $sth->bindParam($key, $val);
}
?>
```


This will fail ($val by value, because bindParam needs &amp;$variable):


```
<?php
foreach ($params as $key => $val) {
    $sth->bindParam($key, $val);
}
?>
```
  

#

Note that when using PDOStatement::bindParam an integer is changed to a string value upon PDOStatement::execute(). (Tested with MySQL). <br><br>This can cause problems when trying to compare values using the === operator.<br><br>Example:<br>

```
<?php
$active = 1;
var_dump($active);
$ps->bindParam(":active", $active, PDO::PARAM_INT);
var_dump($active);
$ps->execute();
var_dump($active);
if ($active === 1) {
    // do something here
    // note: this will fail since $active is now "1"
}
?>
```
<br><br>results in:<br>int(1) <br>int(1) <br>string(1) "1"  

#

There seems to be some confusion about whether you can bind a single value to multiple identical placeholders. For example:<br><br>$sql = "SELECT * FROM user WHERE is_admin = :myValue AND is_deleted = :myValue ";<br><br>$params = array("myValue" =&gt; "0");<br><br>Some users have reported that attempting to bind a single parameter to multiple placeholders yields a parameter mismatch error in PHP version 5.2.0 and earlier. Starting with version 5.2.1, however, this seems to work just fine.<br><br>For details, see bug report 40417:<br>http://bugs.php.net/bug.php?id=40417  

#

Please note, that PDO format numbers according to current locale. So if, locale set number format to something else, that standard that query WILL NOT work properly.<br><br>For example:<br>in Polish locale (pl_PL) proper decimal separator is coma (","), so: 123,45, not 123.45. If we try bind 123.45 to the query, we will end up with coma in the query.<br><br>

```
<?php
setlocale(LC_ALL, 'pl_PL');
$sth = $dbh->prepare('SELECT name FROM products WHERE price < :price');
$sth->bindParam(':price', 123.45, PDO::PARAM_STR);
$sth->execute();
// result:
// SELECT name FROM products WHERE price < '123,45';
?>
```
  

#

Do not try to use the same named parameter twice in a single SQL statement, for example<br><br>

```
<?php
$sql = 'SELECT * FROM some_table WHERE  some_value > :value OR some_value < :value';
$stmt = $dbh->prepare($sql);
$stmt->execute( array( ':value' => 3 ) );
?>
```
<br><br>...this will return no rows and no error -- you must use each parameter once and only once. Apparently this is expected behavior (according to this bug report: http://bugs.php.net/bug.php?id=33886)  because of portability issues.  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.bindparam.php)

**[To root](/README.md)**