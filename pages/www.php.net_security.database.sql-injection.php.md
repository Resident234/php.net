# SQL Injection





The best way has got to be parameterised queries. Then it doesn&apos;t matter what the user types in the data goes to the database as a value. 

A quick search online shows some possibilities in PHP which is great! Even on this site - http://php.net/manual/en/pdo.prepared-statements.php
which also gives the reasons this is good both for security and performance.

  

#



Note that PHP 5 introduced filters that you can use for untrusted user input:
http://us.php.net/manual/en/intro.filter.php

  

#

[Official documentation page](https://www.php.net/manual/en/security.database.sql-injection.php)

**[To root](/README.md)**