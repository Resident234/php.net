# The PDOStatement class



I don&apos;t know why PDOStatement don&apos;t return "execution time" and "found rows" so here I created an extended class of PDOStatement with these attributes.<br><br>Just have to "setAttribute" of PDO&apos;s object to $PDO-&gt;setAttribute(\PDO::ATTR_STATEMENT_CLASS , [&apos;\customs\PDOStatement&apos;, [&amp;$this]]);<br><br>

```
<?php

/**
*
*
*
*/

namespace customs;

/**
*
*
*
*/

final class PDOStatement extends \PDOStatement {

    /**
    *
    *
    *
    */

    protected $PDO = null;
    protected $inputParams = [];
    protected $executionTime = 0;
    protected $resultCount = 0;

    /**
    *
    *
    *
    */

    protected function __construct(PDO &amp;$PDO) {
        $this->PDO = $PDO;
        $this->executionTime = microtime(true);
    }

    /**
    *
    *
    *
    */

    final public function getExecutionError(int $i = 2) {
        $executionError = $this->errorInfo();

        if (isset($executionError[$i]))
            return $executionError[$i];

        return $executionError;
    }

    /**
    *
    *
    *
    */

    final public function getExecutionTime($numberFormat = false, $decPoint = '.', $thousandsSep = ',') {
        if (is_numeric($numberFormat))
            return number_format($this->executionTime, $numberFormat, $decPoint, $thousandsSep);
        
        return $this->executionTime;
    }

    /**
    *
    *
    *
    */

    final public function getResultCount($numberFormat = false, $decPoint = '.', $thousandsSep = ',') {
        if (is_numeric($numberFormat))
            return number_format($this->resultCount, $numberFormat, $decPoint, $thousandsSep);
        
        return $this->resultCount;
    }

    /**
    *
    *
    *
    */

    final public function getLastInsertId() {
        return $this->PDO->lastInsertId();
    }

    /**
    *
    *
    *
    */

    final public function bindValues(array $inputParams) {
        foreach ($this->inputParams = array_values($inputParams) as $i => $value) {
            $varType = is_null($value) ? \PDO::PARAM_NULL : is_bool($value) ? \PDO::PARAM_BOOL : is_int($value) ? \PDO::PARAM_INT : \PDO::PARAM_STR;

            if (!$this->bindValue(++ $i, $value, $varType))
                return false;
        }

        return true;
    }

    /**
    *
    *
    *
    */

    final public function execute($inputParams = null) {
        if ($inputParams)
            $this->inputParams = $inputParams;

        if ($executed = parent::execute($inputParams))
            $this->executionTime = microtime(true) - $this->executionTime;

        return $executed;
    }

    /**
    *
    *
    *
    */

    final public function fetchAll($how = null, $className = null, $ctorArgs = null) {
        $resultSet = parent::fetchAll(... func_get_args());

        if (!empty($resultSet)) {
            $queryString = $this->queryString;
            $inputParams = $this->inputParams;

            if (preg_match('/(.*)?LIMIT/is', $queryString, $match))
                $queryString = $match[1];

            $queryString = sprintf('SELECT COUNT(*) AS T FROM (%s) DT', $queryString);

            if (($placeholders = substr_count($queryString, '?')) &lt; count($inputParams))
                $inputParams = array_slice($inputParams, 0, $placeholders);

            if (($sth = $this->PDO->prepare($queryString)) &amp;&amp; $sth->bindValues($inputParams) &amp;&amp; $sth->execute())
                $this->resultCount = $sth->fetchColumn();
                
            $sth = null;
        }

        return $resultSet;
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.pdostatement.php)

**[To root](/README.md)**