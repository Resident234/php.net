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
        $this-&gt;PDO = $PDO;
        $this-&gt;executionTime = microtime(true);
    }

    /**
    *
    *
    *
    */

    final public function getExecutionError(int $i = 2) {
        $executionError = $this-&gt;errorInfo();

        if (isset($executionError[$i]))
            return $executionError[$i];

        return $executionError;
    }

    /**
    *
    *
    *
    */

    final public function getExecutionTime($numberFormat = false, $decPoint = &apos;.&apos;, $thousandsSep = &apos;,&apos;) {
        if (is_numeric($numberFormat))
            return number_format($this-&gt;executionTime, $numberFormat, $decPoint, $thousandsSep);
        
        return $this-&gt;executionTime;
    }

    /**
    *
    *
    *
    */

    final public function getResultCount($numberFormat = false, $decPoint = &apos;.&apos;, $thousandsSep = &apos;,&apos;) {
        if (is_numeric($numberFormat))
            return number_format($this-&gt;resultCount, $numberFormat, $decPoint, $thousandsSep);
        
        return $this-&gt;resultCount;
    }

    /**
    *
    *
    *
    */

    final public function getLastInsertId() {
        return $this-&gt;PDO-&gt;lastInsertId();
    }

    /**
    *
    *
    *
    */

    final public function bindValues(array $inputParams) {
        foreach ($this-&gt;inputParams = array_values($inputParams) as $i =&gt; $value) {
            $varType = is_null($value) ? \PDO::PARAM_NULL : is_bool($value) ? \PDO::PARAM_BOOL : is_int($value) ? \PDO::PARAM_INT : \PDO::PARAM_STR;

            if (!$this-&gt;bindValue(++ $i, $value, $varType))
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
            $this-&gt;inputParams = $inputParams;

        if ($executed = parent::execute($inputParams))
            $this-&gt;executionTime = microtime(true) - $this-&gt;executionTime;

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
            $queryString = $this-&gt;queryString;
            $inputParams = $this-&gt;inputParams;

            if (preg_match(&apos;/(.*)?LIMIT/is&apos;, $queryString, $match))
                $queryString = $match[1];

            $queryString = sprintf(&apos;SELECT COUNT(*) AS T FROM (%s) DT&apos;, $queryString);

            if (($placeholders = substr_count($queryString, &apos;?&apos;)) &lt; count($inputParams))
                $inputParams = array_slice($inputParams, 0, $placeholders);

            if (($sth = $this-&gt;PDO-&gt;prepare($queryString)) &amp;&amp; $sth-&gt;bindValues($inputParams) &amp;&amp; $sth-&gt;execute())
                $this-&gt;resultCount = $sth-&gt;fetchColumn();
                
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