# PDO::errorCode





Hi,



List containing all SQL-92 SQLSTATE Return Codes:

http://www.unix.org.ua/orelly/java-ent/jenut/ch08_06.htm



Use the following Code to let PDO throw Exceptions (PDOException) on Error.



&lt;?PHP 

$pdo = new PDO (whatever);

$pdo-&gt;setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

try {

&#xA0; &#xA0; $pdo-&gt;exec (&quot;QUERY WITH SYNTAX ERROR&quot;);

} catch (PDOException $e) {

&#xA0; &#xA0; if ($e-&gt;getCode() == &apos;2A000&apos;)

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Syntax Error: &quot;.$e-&gt;getMessage();

}

?>
```




Bye,

&#xA0; Matthias

  

#

[Official documentation page](https://www.php.net/manual/en/pdo.errorcode.php)

**[To root](/README.md)**