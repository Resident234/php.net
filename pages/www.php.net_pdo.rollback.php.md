# PDO::rollBack



Just a quick (and perhaps obvious) note for MySQL users;<br><br>Don&apos;t scratch your head if it isn&apos;t working if you are using a MyISAM table to test the rollbacks with. <br><br>Both rollBack() and beginTransaction() will return TRUE but the rollBack will not happen.<br><br>Convert the table to InnoDB and run the test again.  

---

Here is a way of testing that your transaction has started when using MySQL&apos;s InnoDB tables.  It will fail if you are using MySQL&apos;s MyISAM tables, which do not support transactions but will also not return an error when using them.<br><br>&lt;?<br>// Begin the transaction<br>$dbh-&gt;beginTransaction();<br><br>// To verify that a transaction has started, try to create an (illegal for InnoDB) nested transaction.<br>//    If it works, the first transaction did not start correctly or is unsupported (such as on MyISAM tables)<br>try {<br>    $dbh-&gt;beginTransaction();<br>    die(&apos;Cancelling, Transaction was not properly started&apos;);<br>} catch (PDOException $e) {<br>    print "Transaction is running (because trying another one failed)\n";<br>}<br>?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/pdo.rollback.php)

**[To root](/README.md)**