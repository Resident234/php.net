# mysql_select_db





Be carefull if you are using two databases on the same server at the same time.&#xA0; By default mysql_connect returns the same connection ID for multiple calls with the same server parameters, which means if you do 



```
<?php
&#xA0; $db1 = mysql_connect(...stuff...);
&#xA0; $db2 = mysql_connect(...stuff...);
&#xA0; mysql_select_db(&apos;db1&apos;, $db1);
&#xA0; mysql_select_db(&apos;db2&apos;, $db2); 
?>
```


then $db1 will actually have selected the database &apos;db2&apos;, because the second call to mysql_connect just returned the already opened connection ID !

You have two options here, eiher you have to call mysql_select_db before each query you do, or if you&apos;re using php4.2+ there is a parameter to mysql_connect to force the creation of a new link.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-select-db.php)

**[To root](/README.md)**