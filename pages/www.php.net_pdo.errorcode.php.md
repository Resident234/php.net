# PDO::errorCode



Hi,<br><br>List containing all SQL-92 SQLSTATE Return Codes:<br>http://www.unix.org.ua/orelly/java-ent/jenut/ch08_06.htm<br><br>Use the following Code to let PDO throw Exceptions (PDOException) on Error.<br><br>&lt;?PHP <br>$pdo = new PDO (whatever);<br>$pdo-&gt;setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);<br>try {<br>    $pdo-&gt;exec ("QUERY WITH SYNTAX ERROR");<br>} catch (PDOException $e) {<br>    if ($e-&gt;getCode() == &apos;2A000&apos;)<br>        echo "Syntax Error: ".$e-&gt;getMessage();<br>}<br>?>
```
<br><br>Bye,<br>  Matthias  

#

[Official documentation page](https://www.php.net/manual/en/pdo.errorcode.php)

**[To root](/README.md)**