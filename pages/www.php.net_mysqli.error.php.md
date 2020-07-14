# mysqli::$error



The mysqli_sql_exception class is not available to PHP 5.05<br><br>I used this code to catch errors <br>

```
<?php
$query = "SELECT XXname FROM customer_table ";
$res = $mysqli-&gt;query($query);

if (!$res) {
   printf("Errormessage: %s\n", $mysqli-&gt;error);
}

?>
```

The problem with this is that valid values for $res are: a mysqli_result object , true or false
This doesn&apos;t tell us that there has been an error with the sql used.
If you pass an update statement, false is a valid result if the update fails.

So, a better way is:


```
<?php
$query = "SELECT XXname FROM customer_table ";
$res = $mysqli-&gt;query($query);

if (!$mysqli-&gt;error) {
   printf("Errormessage: %s\n", $mysqli-&gt;error);
}

?>
```


This would output something like:
Unexpected PHP error [mysqli::query() [&lt;a href=&apos;function.query&apos;&gt;function.query&lt;/a&gt;]: (42S22/1054): Unknown column &apos;XXname&apos; in &apos;field list&apos;] severity [E_WARNING] in [G:\database.php] line [249]

Very frustrating as I wanted to also catch the sql error and print out the stack trace. 

A better way is:



```
<?php
mysqli_report(MYSQLI_REPORT_OFF); //Turn off irritating default messages

$mysqli = new mysqli("localhost", "my_user", "my_password", "world");

$query = "SELECT XXname FROM customer_table ";
$res = $mysqli-&gt;query($query);

if ($mysqli-&gt;error) {
    try {    
        throw new Exception("MySQL error $mysqli-&gt;error &lt;br&gt; Query:&lt;br&gt; $query", $msqli-&gt;errno);    
    } catch(Exception $e ) {
        echo "Error No: ".$e-&gt;getCode(). " - ". $e-&gt;getMessage() . "&lt;br &gt;";
        echo nl2br($e-&gt;getTraceAsString());
    }
}

//Do stuff with the result
?>
```
<br>Prints out something like:<br>Error No: 1054<br>Unknown column &apos;XXname&apos; in &apos;field list&apos;<br>Query: <br>SELECT XXname FROM customer_table<br><br>#0 G:\\database.php(251): database-&gt;dbError(&apos;Unknown column ...&apos;, 1054, &apos;getQuery()&apos;, &apos;SELECT XXname F...&apos;)<br>#1 G:\data\WorkSites\1framework5\tests\dbtest.php(29): database-&gt;getString(&apos;SELECT XXname F...&apos;)<br>#2 c:\PHP\includes\simpletest\runner.php(58): testOfDB-&gt;testGetVal()<br>#3 c:\PHP\includes\simpletest\runner.php(96): SimpleInvoker-&gt;invoke(&apos;testGetVal&apos;)<br>#4 c:\PHP\includes\simpletest\runner.php(125): SimpleInvokerDecorator-&gt;invoke(&apos;testGetVal&apos;)<br>#5 c:\PHP\includes\simpletest\runner.php(183): SimpleErrorTrappingInvoker-&gt;invoke(&apos;testGetVal&apos;)<br>#6 c:\PHP\includes\simpletest\simple_test.php(90): SimpleRunner-&gt;run()<br>#7 c:\PHP\includes\simpletest\simple_test.php(498): SimpleTestCase-&gt;run(Object(HtmlReporter))<br>#8 c:\PHP\includes\simpletest\simple_test.php(500): GroupTest-&gt;run(Object(HtmlReporter))<br>#9 G:\all_tests.php(16): GroupTest-&gt;run(Object(HtmlReporter))<br><br>This will actually print out the error, a stack trace and the offending sql statement. Much more helpful when the sql statement is generated somewhere else in the code.  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.error.php)

**[To root](/README.md)**