# mysql_set_charset





I needed to access the database from within one particular webhosting service. Pages are UTF-8 encoded and data received by forms should be inserted into database without changing the encoding. The database is also in UTF-8.

Neither SET character set &apos;utf8&apos; or SET names &apos;utf8&apos; worked properly here, so this workaround made sure all variables are set to utf-8.



```
<?php

// ... (creating a connection to mysql) ...

mysql_query(&quot;SET character_set_results = &apos;utf8&apos;, character_set_client = &apos;utf8&apos;, character_set_connection = &apos;utf8&apos;, character_set_database = &apos;utf8&apos;, character_set_server = &apos;utf8&apos;&quot;, $conn);

$re = mysql_query(&apos;SHOW VARIABLES LIKE &quot;%character_set%&quot;;&apos;)or die(mysql_error());
while ($r = mysql_fetch_assoc($re)) {var_dump ($r); echo &quot;&lt;br /&gt;&quot;;} exit;

?>
```


All important variables are now utf-8 and we can safely use INSERTs or SELECTs with mysql_escape_string($var) without any encoding functions.

  

#



Here&apos;s an example of how to use this feature :

I&apos;m using&#xA0; PHP 5.2.5 from http://oss.oracle.com/projects/php/

I wanted to store arabic characters as UTF8 in the database and as suggested in many of the google results, I tried using

mysql_query(&quot;SET NAMES &apos;utf8&apos;&quot;);

and 

mysql_query(&quot;SET CHARACTER SET utf8 &quot;);

but it did not work.

Using the following, it worked flawlessly

$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;password&apos;);
mysql_set_charset(&apos;utf8&apos;,$link);

Once this is set we need not manually encode the text into utf using utf8_encode() or other functions. The arabic ( or any UTF8 supported ) text can be passed directly to the database and it is automatically converted by PHP.
For eg.


```
<?php
$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;password&apos;);
mysql_set_charset(&apos;utf8&apos;,$link);
$db_selected = mysql_select_db(&apos;emp_feedback&apos;, $link);
if (!$db_selected) { die (&apos;Database access error : &apos; . mysql_error());}
$query = &quot;INSERT INTO feedback ( EmpName, Message ) VALUES (&apos;$_empName&apos;,&apos;$_message&apos;)&quot;;
mysql_query($query) or die(&apos;Error, Feedback insert into database failed&apos;);
?>
```

Note that here $_empName is stored in English while $_message is in Arabic.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-set-charset.php)

**[To root](/README.md)**