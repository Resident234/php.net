# mysql_set_charset



I needed to access the database from within one particular webhosting service. Pages are UTF-8 encoded and data received by forms should be inserted into database without changing the encoding. The database is also in UTF-8.<br><br>Neither SET character set &apos;utf8&apos; or SET names &apos;utf8&apos; worked properly here, so this workaround made sure all variables are set to utf-8.<br><br>

```
<?php

// ... (creating a connection to mysql) ...

mysql_query("SET character_set_results = &apos;utf8&apos;, character_set_client = &apos;utf8&apos;, character_set_connection = &apos;utf8&apos;, character_set_database = &apos;utf8&apos;, character_set_server = &apos;utf8&apos;", $conn);

$re = mysql_query(&apos;SHOW VARIABLES LIKE "%character_set%";&apos;)or die(mysql_error());
while ($r = mysql_fetch_assoc($re)) {var_dump ($r); echo "&lt;br /&gt;";} exit;

?>
```
<br><br>All important variables are now utf-8 and we can safely use INSERTs or SELECTs with mysql_escape_string($var) without any encoding functions.  

#

Here&apos;s an example of how to use this feature :<br><br>I&apos;m using  PHP 5.2.5 from http://oss.oracle.com/projects/php/<br><br>I wanted to store arabic characters as UTF8 in the database and as suggested in many of the google results, I tried using<br><br>mysql_query("SET NAMES &apos;utf8&apos;");<br><br>and <br><br>mysql_query("SET CHARACTER SET utf8 ");<br><br>but it did not work.<br><br>Using the following, it worked flawlessly<br><br>$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;password&apos;);<br>mysql_set_charset(&apos;utf8&apos;,$link);<br><br>Once this is set we need not manually encode the text into utf using utf8_encode() or other functions. The arabic ( or any UTF8 supported ) text can be passed directly to the database and it is automatically converted by PHP.<br>For eg.<br>

```
<?php
$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;password&apos;);
mysql_set_charset(&apos;utf8&apos;,$link);
$db_selected = mysql_select_db(&apos;emp_feedback&apos;, $link);
if (!$db_selected) { die (&apos;Database access error : &apos; . mysql_error());}
$query = "INSERT INTO feedback ( EmpName, Message ) VALUES (&apos;$_empName&apos;,&apos;$_message&apos;)";
mysql_query($query) or die(&apos;Error, Feedback insert into database failed&apos;);
?>
```
<br>Note that here $_empName is stored in English while $_message is in Arabic.  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-set-charset.php)

**[To root](/README.md)**