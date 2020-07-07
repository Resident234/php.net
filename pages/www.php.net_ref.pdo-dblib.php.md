# Microsoft SQL Server and Sybase Functions (PDO_DBLIB)





Hi All, I&apos;wrote a class to connect to MSSQL/Azure databases with Transaction support.

Hope this can help anyone!



```
<?php
/**
*&#xA0; &#xA0; @author&#xA0; &#xA0;&#xA0; Johan Kasselman &lt;johankasselman@live.com&gt;
*&#xA0; &#xA0; @since&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 2015-09-28&#xA0; &#xA0; V1
*
*/

class pdo_dblib_mssql{

&#xA0; &#xA0; private $db;
&#xA0; &#xA0; &#xA0;&#xA0; private $cTransID;
&#xA0; &#xA0; &#xA0;&#xA0; private $childTrans = array();

&#xA0; &#xA0; public function __construct($hostname, $port, $dbname, $username, $pwd){

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;hostname = $hostname;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;port = $port;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;dbname = $dbname;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;username = $username;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;pwd = $pwd;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;connect();
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }

&#xA0; &#xA0; public function beginTransaction(){

&#xA0; &#xA0; &#xA0; &#xA0; $cAlphanum = &quot;AaBbCc0Dd1EeF2fG3gH4hI5iJ6jK7kLlM8mN9nOoPpQqRrSsTtUuVvWwXxYyZz&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;cTransID = &quot;T&quot;.substr(str_shuffle($cAlphanum), 0, 7);

&#xA0; &#xA0; &#xA0; &#xA0; array_unshift($this-&gt;childTrans, $this-&gt;cTransID);

&#xA0; &#xA0; &#xA0; &#xA0; $stmt = $this-&gt;db-&gt;prepare(&quot;BEGIN TRAN [$this-&gt;cTransID];&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; return $stmt-&gt;execute();

&#xA0; &#xA0; }

&#xA0; &#xA0; public function rollBack(){
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; while(count($this-&gt;childTrans) &gt; 0){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cTmp = array_shift($this-&gt;childTrans);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmt = $this-&gt;db-&gt;prepare(&quot;ROLLBACK TRAN [$cTmp];&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmt-&gt;execute();
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return $stmt;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function commit(){

&#xA0; &#xA0; &#xA0; &#xA0; while(count($this-&gt;childTrans) &gt; 0){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cTmp = array_shift($this-&gt;childTrans);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmt = $this-&gt;db-&gt;prepare(&quot;COMMIT TRAN [$cTmp];&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmt-&gt;execute();
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return&#xA0; $stmt;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function close(){
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;db = null;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function connect(){

&#xA0; &#xA0; &#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;db = new PDO (&quot;dblib:host=$this-&gt;hostname:$this-&gt;port;dbname=$this-&gt;dbname&quot;, &quot;$this-&gt;username&quot;, &quot;$this-&gt;pwd&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; } catch (PDOException $e) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;logsys .= &quot;Failed to get DB handle: &quot; . $e-&gt;getMessage() . &quot;\n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}

?>
```



  

#



There is currently little sybase related PDO docs out there. The ones that I found often mention a spec for a dsn that is invalid. Here&apos;s how I am currently connecting to sybase ASE:

1. Compile up freetds http://www.freetds.org on top of open client;
2. Add the PDO and PD_DBLIB modules to php 5 as per the documentation; Note: I&apos;m currently using the PDO-beta and PDO_DBLIB-beta;
3. Check mods installed ok using &quot;pear list&quot; and &quot;php -m&quot;;

The documentation often says to use &quot;sybase:&quot; as your DSN, this doesn&apos;t work. Use &quot;dblib:&quot; instead. Here&apos;s an example:



```
<?php
&#xA0; try {
&#xA0; &#xA0; $hostname = &quot;myhost&quot;;
&#xA0; &#xA0; $port = 10060;
&#xA0; &#xA0; $dbname = &quot;tempdb&quot;;
&#xA0; &#xA0; $username = &quot;dbuser&quot;;
&#xA0; &#xA0; $pw = &quot;password&quot;;
&#xA0; &#xA0; $dbh = new PDO (&quot;dblib:host=$hostname:$port;dbname=$dbname&quot;,&quot;$username&quot;,&quot;$pw&quot;);
&#xA0; } catch (PDOException $e) {
&#xA0; &#xA0; echo &quot;Failed to get DB handle: &quot; . $e-&gt;getMessage() . &quot;\n&quot;;
&#xA0; &#xA0; exit;
&#xA0; }
&#xA0; $stmt = $dbh-&gt;prepare(&quot;select name from master..sysdatabases where name = db_name()&quot;);
&#xA0; $stmt-&gt;execute();
&#xA0; while ($row = $stmt-&gt;fetch()) {
&#xA0; &#xA0; print_r($row);
&#xA0; }
&#xA0; unset($dbh); unset($stmt);
?>
```


Hope this helps.

  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-dblib.php)

**[To root](/README.md)**