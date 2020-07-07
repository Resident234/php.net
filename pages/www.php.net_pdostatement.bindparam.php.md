# PDOStatement::bindParam





I know this has been said before but I&apos;ll write a note on it too because I think it&apos;s important to keep in mind:

If you use PDO bindParam to do a search with a LIKE condition you cannot put the percentages and quotes to the param placeholder &apos;%:keyword%&apos;.

This is WRONG:
&quot;SELECT * FROM `users` WHERE `firstname` LIKE &apos;%:keyword%&apos;&quot;;

The CORRECT solution is to leave clean the placeholder like this:
&quot;SELECT * FROM `users` WHERE `firstname` LIKE :keyword&quot;;

And then add the percentages to the php variable where you store the keyword:
$keyword = &quot;%&quot;.$keyword.&quot;%&quot;;

And finally the quotes will be automatically added by PDO when executing the query so you don&apos;t have to worry about them.

So the full example would be:


```
<?php
// Get the keyword from query string
$keyword = $_GET[&apos;keyword&apos;];
// Prepare the command
$sth = $dbh-&gt;prepare(&apos;SELECT * FROM `users` WHERE `firstname` LIKE :keyword&apos;);
// Put the percentage sing on the keyword
$keyword = &quot;%&quot;.$keyword.&quot;%&quot;;
// Bind the parameter
$sth-&gt;bindParam(&apos;:keyword&apos;, $keyword, PDO::PARAM_STR);
?>
```



  

#



This works ($val by reference):


```
<?php
foreach ($params as $key =&gt; &amp;$val) {
&#xA0; &#xA0; $sth-&gt;bindParam($key, $val);
}
?>
```


This will fail ($val by value, because bindParam needs &amp;$variable):


```
<?php
foreach ($params as $key =&gt; $val) {
&#xA0; &#xA0; $sth-&gt;bindParam($key, $val);
}
?>
```



  

#



Note that when using PDOStatement::bindParam an integer is changed to a string value upon PDOStatement::execute(). (Tested with MySQL). 

This can cause problems when trying to compare values using the === operator.

Example:


```
<?php
$active = 1;
var_dump($active);
$ps-&gt;bindParam(&quot;:active&quot;, $active, PDO::PARAM_INT);
var_dump($active);
$ps-&gt;execute();
var_dump($active);
if ($active === 1) {
&#xA0; &#xA0; // do something here
&#xA0; &#xA0; // note: this will fail since $active is now &quot;1&quot;
}
?>
```


results in:
int(1) 
int(1) 
string(1) &quot;1&quot;

  

#



There seems to be some confusion about whether you can bind a single value to multiple identical placeholders. For example:

$sql = &quot;SELECT * FROM user WHERE is_admin = :myValue AND is_deleted = :myValue &quot;;

$params = array(&quot;myValue&quot; =&gt; &quot;0&quot;);

Some users have reported that attempting to bind a single parameter to multiple placeholders yields a parameter mismatch error in PHP version 5.2.0 and earlier. Starting with version 5.2.1, however, this seems to work just fine.

For details, see bug report 40417:
http://bugs.php.net/bug.php?id=40417

  

#



Please note, that PDO format numbers according to current locale. So if, locale set number format to something else, that standard that query WILL NOT work properly.

For example:
in Polish locale (pl_PL) proper decimal separator is coma (&quot;,&quot;), so: 123,45, not 123.45. If we try bind 123.45 to the query, we will end up with coma in the query.



```
<?php
setlocale(LC_ALL, &apos;pl_PL&apos;);
$sth = $dbh-&gt;prepare(&apos;SELECT name FROM products WHERE price &lt; :price&apos;);
$sth-&gt;bindParam(&apos;:price&apos;, 123.45, PDO::PARAM_STR);
$sth-&gt;execute();
// result:
// SELECT name FROM products WHERE price &lt; &apos;123,45&apos;;
?>
```



  

#



Do not try to use the same named parameter twice in a single SQL statement, for example





```
<?php

$sql = &apos;SELECT * FROM some_table WHERE&#xA0; some_value &gt; :value OR some_value &lt; :value&apos;;

$stmt = $dbh-&gt;prepare($sql);

$stmt-&gt;execute( array( &apos;:value&apos; =&gt; 3 ) );

?>
```




...this will return no rows and no error -- you must use each parameter once and only once. Apparently this is expected behavior (according to this bug report: http://bugs.php.net/bug.php?id=33886)&#xA0; because of portability issues.

  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.bindparam.php)

**[To root](/README.md)**