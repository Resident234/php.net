# PDO::beginTransaction





The nested transaction example here is great, but it&apos;s missing a key piece of the puzzle.&#xA0; Commits will commit everything, I only wanted commits to actually commit when the outermost commit has been completed.&#xA0; This can be done in InnoDB with savepoints.



```
<?php

class Database extends PDO
{

&#xA0; &#xA0; protected $transactionCount = 0;

&#xA0; &#xA0; public function beginTransaction()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!$this-&gt;transactionCounter++) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return parent::beginTransaction();
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;exec(&apos;SAVEPOINT trans&apos;.$this-&gt;transactionCounter);
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;transactionCounter &gt;= 0;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function commit()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!--$this-&gt;transactionCounter) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return parent::commit();
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;transactionCounter &gt;= 0;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function rollback()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (--$this-&gt;transactionCounter) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;exec(&apos;ROLLBACK TO trans&apos;.$this-&gt;transactionCounter + 1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return parent::rollback();
&#xA0; &#xA0; }
&#xA0; &#xA0; 
}


  

#



You can generate problems with nested beginTransaction and commit calls.

example:



beginTransaction()

do imprortant stuff

call method

&#xA0; &#xA0; beginTransaction()

&#xA0; &#xA0; basic stuff 1

&#xA0; &#xA0; basic stuff 2

&#xA0; &#xA0; commit()

do most important stuff

commit()



Won&apos;t work and is dangerous since you could close your transaction too early with the nested commit().



There is no need to mess you code and pass like a bool which indicate if transaction is already running. You could just overload the beginTransaction() and commit() in your PDO wrapper like this:





```
<?php

class Database extends \\PDO

{

&#xA0; &#xA0; protected $transactionCounter = 0;

&#xA0; &#xA0; function beginTransaction()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; if(!$this-&gt;transactionCounter++)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return parent::beginTransaction();

&#xA0; &#xA0; &#xA0;&#xA0; return $this-&gt;transactionCounter &gt;= 0;

&#xA0; &#xA0; }



&#xA0; &#xA0; function commit()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0;&#xA0; if(!--$this-&gt;transactionCounter)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return parent::commit();

&#xA0; &#xA0; &#xA0;&#xA0; return $this-&gt;transactionCounter &gt;= 0;

&#xA0; &#xA0; }



&#xA0; &#xA0; function rollback()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;transactionCounter &gt;= 0)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;transactionCounter = 0;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return parent::rollback();

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;transactionCounter = 0;

&#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; }

//...

}

?>
```



  

#



In response to &quot;Anonymous / 20-Dec-2007 03:04&quot;

You could also extend the PDO class and hold a private flag to check if a transaction is already started.

class MyPDO extends PDO {
&#xA0;&#xA0; protected $hasActiveTransaction = false;

&#xA0;&#xA0; function beginTransaction () {
&#xA0; &#xA0; &#xA0; if ( $this-&gt;hasActiveTransaction ) {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return false;
&#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;hasActiveTransaction = parent::beginTransaction ();
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return $this-&gt;hasActiveTransaction;
&#xA0; &#xA0; &#xA0; }
&#xA0;&#xA0; }

&#xA0;&#xA0; function commit () {
&#xA0; &#xA0; &#xA0; parent::commit ();
&#xA0; &#xA0; &#xA0; $this-&gt;hasActiveTransaction = false;
&#xA0;&#xA0; }

&#xA0;&#xA0; function rollback () {
&#xA0; &#xA0; &#xA0; parent::rollback ();
&#xA0; &#xA0; &#xA0; $this-&gt;hasActiveTransaction = false;
&#xA0;&#xA0; }

}

  

#

[Official documentation page](https://www.php.net/manual/en/pdo.begintransaction.php)

**[To root](/README.md)**