# PDOStatement::execute



Hopefully this saves time for folks: one should use $count = $stmt-&gt;rowCount() after $stmt-&gt;execute() in order to really determine if any an operation such as &apos; update &apos; or &apos; replace &apos; did succeed i.e. changed some data.<br><br>Jean-Lou Dupont.  

#

Note that you must<br>- EITHER pass all values to bind in an array to PDOStatement::execute()<br>- OR bind every value before with PDOStatement::bindValue(), then call PDOStatement::execute() with *no* parameter (not even "array()"!).<br>Passing an array (empty or not) to execute() will "erase" and replace any previous bindings (and can lead to, e.g. with MySQL, "SQLSTATE[HY000]: General error: 2031" (CR_PARAMS_NOT_BOUND) if you passed an empty array).<br><br>Thus the following function is incorrect in case the prepared statement has been "bound" before:<br><br>

```
<?php
function customExecute(PDOStatement &amp;$sth, $params = NULL) {
    return $sth-&gt;execute($params);
}
?>
```


and should therefore be replaced by something like:



```
<?php
function customExecute(PDOStatement &amp;$sth, array $params = array()) {
    if (empty($params))
        return $sth-&gt;execute();
    return $sth-&gt;execute($params);
}
?>
```
<br><br>Also note that PDOStatement::execute() doesn&apos;t require $input_parameters to be an array.<br><br>(of course, do not use it as is ^^).  

#

An array of insert values (named parameters) don&apos;t need the prefixed colon als key-value to work.<br><br>

```
<?php
/* Execute a prepared statement by passing an array of insert values */
$calories = 150;
$colour = &apos;red&apos;;
$sth = $dbh-&gt;prepare(&apos;SELECT name, colour, calories
   FROM fruit
   WHERE calories &lt; :calories AND colour = :colour&apos;);
// instead of:
//     $sth-&gt;execute(array(&apos;:calories&apos; =&gt; $calories, &apos;:colour&apos; =&gt; $colour));
// this works fine, too:
$sth-&gt;execute(array(&apos;calories&apos; =&gt; $calories, &apos;colour&apos; =&gt; $colour));
?>
```
<br><br>This allows to use "regular" assembled hash-tables (arrays).<br>That realy does make sense!  

#

simplified $placeholder form <br><br>

```
<?php<br><br>$data = [&apos;a&apos;=&gt;&apos;foo&apos;,&apos;b&apos;=&gt;&apos;bar&apos;];<br><br>$keys = array_keys($data);<br>$fields = &apos;`&apos;.implode(&apos;`, `&apos;,$keys).&apos;`&apos;;<br><br>#here is my way <br>$placeholder = substr(str_repeat(&apos;?,&apos;,count($keys)),0,-1);<br><br>$pdo-&gt;prepare("INSERT INTO `baz`($fields) VALUES($placeholder)")-&gt;execute(array_values($data));  

#

When using a prepared statement to execute multiple inserts (such as in a loop etc), under sqlite the performance is dramatically improved by wrapping the loop in a transaction.<br><br>I have an application that routinely inserts 30-50,000 records at a time.  Without the transaction it was taking over 150 seconds, and with it only 3.<br><br>This may affect other implementations as well, and I am sure it is something that affects all databases to some extent, but I can only test with PDO sqlite.<br><br>e.g.<br><br>

```
<?php
$data = array(
  array(&apos;name&apos; =&gt; &apos;John&apos;, &apos;age&apos; =&gt; &apos;25&apos;),
  array(&apos;name&apos; =&gt; &apos;Wendy&apos;, &apos;age&apos; =&gt; &apos;32&apos;)
);

try {
  $pdo = new PDO(&apos;sqlite:myfile.sqlite&apos;);
}

catch(PDOException $e) {
  die(&apos;Unable to open database connection&apos;);
}

$insertStatement = $pdo-&gt;prepare(&apos;insert into mytable (name, age) values (:name, :age)&apos;);

// start transaction
$pdo-&gt;beginTransaction();

foreach($data as &amp;$row) {
  $insertStatement-&gt;execute($row);
}

// end transaction
$pdo-&gt;commit();

?>
```
<br><br>[EDITED BY sobak: typofixes by Pere submitted on 12-Sep-2014 01:07]  

#

When passing an array of values to execute when your query contains question marks, note that the array must be keyed numerically from zero. If it is not, run array_values() on it to force the array to be re-keyed.<br><br>

```
<?php
$anarray = array(42 =&gt; "foo", 101 =&gt; "bar");
$statement = $dbo-&gt;prepare("SELECT * FROM table WHERE col1 = ? AND col2 = ?");

//This will not work
$statement-&gt;execute($anarray);

//Do this to make it work
$statement-&gt;execute(array_values($anarray));
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.execute.php)

**[To root](/README.md)**