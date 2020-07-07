# PDO::rollBack





Just a quick (and perhaps obvious) note for MySQL users;

Don&apos;t scratch your head if it isn&apos;t working if you are using a MyISAM table to test the rollbacks with. 

Both rollBack() and beginTransaction() will return TRUE but the rollBack will not happen.

Convert the table to InnoDB and run the test again.

  

#



Here is a way of testing that your transaction has started when using MySQL&apos;s InnoDB tables.&#xA0; It will fail if you are using MySQL&apos;s MyISAM tables, which do not support transactions but will also not return an error when using them.

&lt;?
// Begin the transaction
$dbh-&gt;beginTransaction();

// To verify that a transaction has started, try to create an (illegal for InnoDB) nested transaction.
//&#xA0; &#xA0; If it works, the first transaction did not start correctly or is unsupported (such as on MyISAM tables)
try {
&#xA0; &#xA0; $dbh-&gt;beginTransaction();
&#xA0; &#xA0; die(&apos;Cancelling, Transaction was not properly started&apos;);
} catch (PDOException $e) {
&#xA0; &#xA0; print &quot;Transaction is running (because trying another one failed)\n&quot;;
}
?>
```


  

#

[Official documentation page](https://www.php.net/manual/en/pdo.rollback.php)

**[To root](/README.md)**