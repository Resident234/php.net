# SQL Injection




<div class="phpcode"><span class="html">
The best way has got to be parameterised queries. Then it doesn&apos;t matter what the user types in the data goes to the database as a value. <br><br>A quick search online shows some possibilities in PHP which is great! Even on this site - <a href="http://php.net/manual/en/pdo.prepared-statements.php" rel="nofollow" target="_blank">http://php.net/manual/en/pdo.prepared-statements.php</a><br>which also gives the reasons this is good both for security and performance.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that PHP 5 introduced filters that you can use for untrusted user input:<br><a href="http://us.php.net/manual/en/intro.filter.php" rel="nofollow" target="_blank">http://us.php.net/manual/en/intro.filter.php</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.database.sql-injection.php)

**[To root](/)**