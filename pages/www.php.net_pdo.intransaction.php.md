# PDO::inTransaction




<div class="phpcode"><span class="html">
Exceptions regarding existing active transactions were thrown while I was almost certain sufficient checks were in place.<br>However, I quickly found out that a strict boolean comparison to PDO::inTransaction() was failing.<br><br>Using var_dump I learned that this function was returning integers, not boolean values.<br><br>var_dump(PDO::inTransaction()); // int(1) || int(0)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.intransaction.php)

**[To root](/README.md)**