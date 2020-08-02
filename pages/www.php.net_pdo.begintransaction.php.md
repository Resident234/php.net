# PDO::beginTransaction



The nested transaction example here is great, but it&apos;s missing a key piece of the puzzle.  Commits will commit everything, I only wanted commits to actually commit when the outermost commit has been completed.  This can be done in InnoDB with savepoints.<br><br>

```
<?php

class Database extends PDO
{

    protected $transactionCount = 0;

    public function beginTransaction()
    {
        if (!$this->transactionCounter++) {
            return parent::beginTransaction();
        }
        $this->exec('SAVEPOINT trans'.$this->transactionCounter);
        return $this->transactionCounter >= 0;
    }

    public function commit()
    {
        if (!--$this->transactionCounter) {
            return parent::commit();
        }
        return $this->transactionCounter >= 0;
    }

    public function rollback()
    {
        if (--$this->transactionCounter) {
            $this->exec('ROLLBACK TO trans'.$this->transactionCounter + 1);
            return true;
        }
        return parent::rollback();
    }
    
}?>
```
  

---

You can generate problems with nested beginTransaction and commit calls.<br>example:<br><br>beginTransaction()<br>do imprortant stuff<br>call method<br>    beginTransaction()<br>    basic stuff 1<br>    basic stuff 2<br>    commit()<br>do most important stuff<br>commit()<br><br>Won&apos;t work and is dangerous since you could close your transaction too early with the nested commit().<br><br>There is no need to mess you code and pass like a bool which indicate if transaction is already running. You could just overload the beginTransaction() and commit() in your PDO wrapper like this:<br><br>

```
<?php
class Database extends \\PDO
{
    protected $transactionCounter = 0;
    function beginTransaction()
    {
        if(!$this->transactionCounter++)
            return parent::beginTransaction();
       return $this->transactionCounter >= 0;
    }

    function commit()
    {
       if(!--$this->transactionCounter)
           return parent::commit();
       return $this->transactionCounter >= 0;
    }

    function rollback()
    {
        if($this->transactionCounter >= 0)
        {
            $this->transactionCounter = 0;
            return parent::rollback();
        }
        $this->transactionCounter = 0;
        return false;
    }
//...
}
?>
```
  

---

In response to "Anonymous / 20-Dec-2007 03:04"<br><br>You could also extend the PDO class and hold a private flag to check if a transaction is already started.<br><br>class MyPDO extends PDO {<br>   protected $hasActiveTransaction = false;<br><br>   function beginTransaction () {<br>      if ( $this-&gt;hasActiveTransaction ) {<br>         return false;<br>      } else {<br>         $this-&gt;hasActiveTransaction = parent::beginTransaction ();<br>         return $this-&gt;hasActiveTransaction;<br>      }<br>   }<br><br>   function commit () {<br>      parent::commit ();<br>      $this-&gt;hasActiveTransaction = false;<br>   }<br><br>   function rollback () {<br>      parent::rollback ();<br>      $this-&gt;hasActiveTransaction = false;<br>   }<br><br>}  

---

[Official documentation page](https://www.php.net/manual/en/pdo.begintransaction.php)

**[To root](/README.md)**