# PDOStatement::execute



Hopefully this saves time for folks: one should use $count = $stmt-&gt;rowCount() after $stmt-&gt;execute() in order to really determine if any an operation such as &apos; update &apos; or &apos; replace &apos; did succeed i.e. changed some data.<br><br>Jean-Lou Dupont.  

---

Note that you must<br>- EITHER pass all values to bind in an array to PDOStatement::execute()<br>- OR bind every value before with PDOStatement::bindValue(), then call PDOStatement::execute() with *no* parameter (not even "array()"!).<br>Passing an array (empty or not) to execute() will "erase" and replace any previous bindings (and can lead to, e.g. with MySQL, "SQLSTATE[HY000]: General error: 2031" (CR_PARAMS_NOT_BOUND) if you passed an empty array).<br><br>Thus the following function is incorrect in case the prepared statement has been "bound" before:<br><br>

```
<?php
function customExecute(PDOStatement &amp;$sth, $params = NULL) {
    return $sth->execute($params);
}
?>
```


and should therefore be replaced by something like:



```
<?php
function customExecute(PDOStatement &amp;$sth, array $params = array()) {
    if (empty($params))
        return $sth->execute();
    return $sth->execute($params);
}
?>
```
<br><br>Also note that PDOStatement::execute() doesn&apos;t require $input_parameters to be an array.<br><br>(of course, do not use it as is ^^).  

---

An array of insert values (named parameters) don&apos;t need the prefixed colon als key-value to work.<br><br>

```
<?php
/* Execute a prepared statement by passing an array of insert values */
$calories = 150;
$colour = 'red';
$sth = $dbh->prepare('SELECT name, colour, calories
   FROM fruit
   WHERE calories < :calories AND colour = :colour');
// instead of:
//     $sth->execute(array(':calories' => $calories, ':colour' => $colour));
// this works fine, too:
$sth->execute(array('calories' => $calories, 'colour' => $colour));
?>
```
<br><br>This allows to use "regular" assembled hash-tables (arrays).<br>That realy does make sense!  

---

simplified $placeholder form <br><br>

```
<?php

$data = ['a'=>'foo','b'=>'bar'];

$keys = array_keys($data);
$fields = '`'.implode('`, `',$keys).'`';

#here is my way 
$placeholder = substr(str_repeat('?,',count($keys)),0,-1);

$pdo->prepare("INSERT INTO `baz`($fields) VALUES($placeholder)")->execute(array_values($data));?>
```
  

---

When using a prepared statement to execute multiple inserts (such as in a loop etc), under sqlite the performance is dramatically improved by wrapping the loop in a transaction.<br><br>I have an application that routinely inserts 30-50,000 records at a time.  Without the transaction it was taking over 150 seconds, and with it only 3.<br><br>This may affect other implementations as well, and I am sure it is something that affects all databases to some extent, but I can only test with PDO sqlite.<br><br>e.g.<br><br>

```
<?php
$data = array(
  array('name' => 'John', 'age' => '25'),
  array('name' => 'Wendy', 'age' => '32')
);

try {
  $pdo = new PDO('sqlite:myfile.sqlite');
}

catch(PDOException $e) {
  die('Unable to open database connection');
}

$insertStatement = $pdo->prepare('insert into mytable (name, age) values (:name, :age)');

// start transaction
$pdo->beginTransaction();

foreach($data as &amp;$row) {
  $insertStatement->execute($row);
}

// end transaction
$pdo->commit();

?>
```
<br><br>[EDITED BY sobak: typofixes by Pere submitted on 12-Sep-2014 01:07]  

---

When passing an array of values to execute when your query contains question marks, note that the array must be keyed numerically from zero. If it is not, run array_values() on it to force the array to be re-keyed.<br><br>

```
<?php
$anarray = array(42 => "foo", 101 => "bar");
$statement = $dbo->prepare("SELECT * FROM table WHERE col1 = ? AND col2 = ?");

//This will not work
$statement->execute($anarray);

//Do this to make it work
$statement->execute(array_values($anarray));
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/pdostatement.execute.php)

**[To root](/README.md)**