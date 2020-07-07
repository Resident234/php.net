# PDO::inTransaction





Exceptions regarding existing active transactions were thrown while I was almost certain sufficient checks were in place.
However, I quickly found out that a strict boolean comparison to PDO::inTransaction() was failing.

Using var_dump I learned that this function was returning integers, not boolean values.

var_dump(PDO::inTransaction()); // int(1) || int(0)

  

#

[Official documentation page](https://www.php.net/manual/en/pdo.intransaction.php)

**[To root](/README.md)**