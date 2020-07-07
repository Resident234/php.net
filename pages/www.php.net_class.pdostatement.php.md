# The PDOStatement class





I don&apos;t know why PDOStatement don&apos;t return &quot;execution time&quot; and &quot;found rows&quot; so here I created an extended class of PDOStatement with these attributes.

Just have to &quot;setAttribute&quot; of PDO&apos;s object to $PDO-&gt;setAttribute(\PDO::ATTR_STATEMENT_CLASS , [&apos;\customs\PDOStatement&apos;, [&amp;$this]]);



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

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; protected $PDO = null;
&#xA0; &#xA0; protected $inputParams = [];
&#xA0; &#xA0; protected $executionTime = 0;
&#xA0; &#xA0; protected $resultCount = 0;

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; protected function __construct(PDO &amp;$PDO) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;PDO = $PDO;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;executionTime = microtime(true);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function getExecutionError(int $i = 2) {
&#xA0; &#xA0; &#xA0; &#xA0; $executionError = $this-&gt;errorInfo();

&#xA0; &#xA0; &#xA0; &#xA0; if (isset($executionError[$i]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $executionError[$i];

&#xA0; &#xA0; &#xA0; &#xA0; return $executionError;
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function getExecutionTime($numberFormat = false, $decPoint = &apos;.&apos;, $thousandsSep = &apos;,&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; if (is_numeric($numberFormat))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return number_format($this-&gt;executionTime, $numberFormat, $decPoint, $thousandsSep);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;executionTime;
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function getResultCount($numberFormat = false, $decPoint = &apos;.&apos;, $thousandsSep = &apos;,&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; if (is_numeric($numberFormat))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return number_format($this-&gt;resultCount, $numberFormat, $decPoint, $thousandsSep);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;resultCount;
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function getLastInsertId() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;PDO-&gt;lastInsertId();
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function bindValues(array $inputParams) {
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($this-&gt;inputParams = array_values($inputParams) as $i =&gt; $value) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $varType = is_null($value) ? \PDO::PARAM_NULL : is_bool($value) ? \PDO::PARAM_BOOL : is_int($value) ? \PDO::PARAM_INT : \PDO::PARAM_STR;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$this-&gt;bindValue(++ $i, $value, $varType))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function execute($inputParams = null) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($inputParams)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;inputParams = $inputParams;

&#xA0; &#xA0; &#xA0; &#xA0; if ($executed = parent::execute($inputParams))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;executionTime = microtime(true) - $this-&gt;executionTime;

&#xA0; &#xA0; &#xA0; &#xA0; return $executed;
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; *
&#xA0; &#xA0; */

&#xA0; &#xA0; final public function fetchAll($how = null, $className = null, $ctorArgs = null) {
&#xA0; &#xA0; &#xA0; &#xA0; $resultSet = parent::fetchAll(... func_get_args());

&#xA0; &#xA0; &#xA0; &#xA0; if (!empty($resultSet)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $queryString = $this-&gt;queryString;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $inputParams = $this-&gt;inputParams;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(&apos;/(.*)?LIMIT/is&apos;, $queryString, $match))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $queryString = $match[1];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $queryString = sprintf(&apos;SELECT COUNT(*) AS T FROM (%s) DT&apos;, $queryString);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (($placeholders = substr_count($queryString, &apos;?&apos;)) &lt; count($inputParams))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $inputParams = array_slice($inputParams, 0, $placeholders);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (($sth = $this-&gt;PDO-&gt;prepare($queryString)) &amp;&amp; $sth-&gt;bindValues($inputParams) &amp;&amp; $sth-&gt;execute())
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;resultCount = $sth-&gt;fetchColumn();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $sth = null;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return $resultSet;
&#xA0; &#xA0; }
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.pdostatement.php)

**[To root](/README.md)**