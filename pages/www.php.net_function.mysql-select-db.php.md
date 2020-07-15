# mysql_select_db



Be carefull if you are using two databases on the same server at the same time.  By default mysql_connect returns the same connection ID for multiple calls with the same server parameters, which means if you do <br><br>

```
<?php
  $db1 = mysql_connect(...stuff...);
  $db2 = mysql_connect(...stuff...);
  mysql_select_db('db1', $db1);
  mysql_select_db('db2', $db2); 
?>
```
<br><br>then $db1 will actually have selected the database &apos;db2&apos;, because the second call to mysql_connect just returned the already opened connection ID !<br><br>You have two options here, eiher you have to call mysql_select_db before each query you do, or if you&apos;re using php4.2+ there is a parameter to mysql_connect to force the creation of a new link.  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-select-db.php)

**[To root](/README.md)**