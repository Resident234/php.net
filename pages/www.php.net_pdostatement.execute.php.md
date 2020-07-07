# PDOStatement::execute





Hopefully this saves time for folks: one should use $count = $stmt-&gt;rowCount() after $stmt-&gt;execute() in order to really determine if any an operation such as &apos; update &apos; or &apos; replace &apos; did succeed i.e. changed some data.

Jean-Lou Dupont.

  

#



Note that you must

- EITHER pass all values to bind in an array to PDOStatement::execute()

- OR bind every value before with PDOStatement::bindValue(), then call PDOStatement::execute() with *no* parameter (not even &quot;array()&quot;!).

Passing an array (empty or not) to execute() will &quot;erase&quot; and replace any previous bindings (and can lead to, e.g. with MySQL, &quot;SQLSTATE[HY000]: General error: 2031&quot; (CR_PARAMS_NOT_BOUND) if you passed an empty array).



Thus the following function is incorrect in case the prepared statement has been &quot;bound&quot; before:





```
<?php

function customExecute(PDOStatement &amp;$sth, $params = NULL) {

&#xA0; &#xA0; return $sth-&gt;execute($params);

}

?>
```




and should therefore be replaced by something like:





```
<?php

function customExecute(PDOStatement &amp;$sth, array $params = array()) {

&#xA0; &#xA0; if (empty($params))

&#xA0; &#xA0; &#xA0; &#xA0; return $sth-&gt;execute();

&#xA0; &#xA0; return $sth-&gt;execute($params);

}

?>
```




Also note that PDOStatement::execute() doesn&apos;t require $input_parameters to be an array.



(of course, do not use it as is ^^).

  

#



An array of insert values (named parameters) don&apos;t need the prefixed colon als key-value to work.



```
<?php
/* Execute a prepared statement by passing an array of insert values */
$calories = 150;
$colour = &apos;red&apos;;
$sth = $dbh-&gt;prepare(&apos;SELECT name, colour, calories
&#xA0;&#xA0; FROM fruit
&#xA0;&#xA0; WHERE calories &lt; :calories AND colour = :colour&apos;);
// instead of:
//&#xA0; &#xA0;&#xA0; $sth-&gt;execute(array(&apos;:calories&apos; =&gt; $calories, &apos;:colour&apos; =&gt; $colour));
// this works fine, too:
$sth-&gt;execute(array(&apos;calories&apos; =&gt; $calories, &apos;colour&apos; =&gt; $colour));
?>
```


This allows to use &quot;regular&quot; assembled hash-tables (arrays).
That realy does make sense!

  

#



simplified $placeholder form 





```
<?php



$data = [&apos;a&apos;=&gt;&apos;foo&apos;,&apos;b&apos;=&gt;&apos;bar&apos;];



$keys = array_keys($data);

$fields = &apos;`&apos;.implode(&apos;`, `&apos;,$keys).&apos;`&apos;;



#here is my way 

$placeholder = substr(str_repeat(&apos;?,&apos;,count($keys)),0,-1);



$pdo-&gt;prepare(&quot;INSERT INTO `baz`($fields) VALUES($placeholder)&quot;)-&gt;execute(array_values($data));


  

#



When using a prepared statement to execute multiple inserts (such as in a loop etc), under sqlite the performance is dramatically improved by wrapping the loop in a transaction.



I have an application that routinely inserts 30-50,000 records at a time.&#xA0; Without the transaction it was taking over 150 seconds, and with it only 3.



This may affect other implementations as well, and I am sure it is something that affects all databases to some extent, but I can only test with PDO sqlite.



e.g.





```
<?php

$data = array(

&#xA0; array(&apos;name&apos; =&gt; &apos;John&apos;, &apos;age&apos; =&gt; &apos;25&apos;),

&#xA0; array(&apos;name&apos; =&gt; &apos;Wendy&apos;, &apos;age&apos; =&gt; &apos;32&apos;)

);



try {

&#xA0; $pdo = new PDO(&apos;sqlite:myfile.sqlite&apos;);

}



catch(PDOException $e) {

&#xA0; die(&apos;Unable to open database connection&apos;);

}



$insertStatement = $pdo-&gt;prepare(&apos;insert into mytable (name, age) values (:name, :age)&apos;);



// start transaction

$pdo-&gt;beginTransaction();



foreach($data as &amp;$row) {

&#xA0; $insertStatement-&gt;execute($row);

}



// end transaction

$pdo-&gt;commit();



?>
```




[EDITED BY sobak: typofixes by Pere submitted on 12-Sep-2014 01:07]

  

#



When passing an array of values to execute when your query contains question marks, note that the array must be keyed numerically from zero. If it is not, run array_values() on it to force the array to be re-keyed.



```
<?php
$anarray = array(42 =&gt; &quot;foo&quot;, 101 =&gt; &quot;bar&quot;);
$statement = $dbo-&gt;prepare(&quot;SELECT * FROM table WHERE col1 = ? AND col2 = ?&quot;);

//This will not work
$statement-&gt;execute($anarray);

//Do this to make it work
$statement-&gt;execute(array_values($anarray));
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.execute.php)

**[To root](/README.md)**