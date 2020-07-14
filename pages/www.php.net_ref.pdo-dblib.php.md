# Microsoft SQL Server and Sybase Functions (PDO_DBLIB)



Hi All, I&apos;wrote a class to connect to MSSQL/Azure databases with Transaction support.<br><br>Hope this can help anyone!<br><br>

```
<?php
/**
*    @author     Johan Kasselman &lt;johankasselman@live.com&gt;
*    @since         2015-09-28    V1
*
*/

class pdo_dblib_mssql{

    private $db;
       private $cTransID;
       private $childTrans = array();

    public function __construct($hostname, $port, $dbname, $username, $pwd){

        $this-&gt;hostname = $hostname;
        $this-&gt;port = $port;
        $this-&gt;dbname = $dbname;
        $this-&gt;username = $username;
        $this-&gt;pwd = $pwd;

        $this-&gt;connect();
        
    }

    public function beginTransaction(){

        $cAlphanum = "AaBbCc0Dd1EeF2fG3gH4hI5iJ6jK7kLlM8mN9nOoPpQqRrSsTtUuVvWwXxYyZz";
        $this-&gt;cTransID = "T".substr(str_shuffle($cAlphanum), 0, 7);

        array_unshift($this-&gt;childTrans, $this-&gt;cTransID);

        $stmt = $this-&gt;db-&gt;prepare("BEGIN TRAN [$this-&gt;cTransID];");
        return $stmt-&gt;execute();

    }

    public function rollBack(){
        
        while(count($this-&gt;childTrans) &gt; 0){
            $cTmp = array_shift($this-&gt;childTrans);
            $stmt = $this-&gt;db-&gt;prepare("ROLLBACK TRAN [$cTmp];");
            $stmt-&gt;execute();
        }

        return $stmt;
    }

    public function commit(){

        while(count($this-&gt;childTrans) &gt; 0){
            $cTmp = array_shift($this-&gt;childTrans);
            $stmt = $this-&gt;db-&gt;prepare("COMMIT TRAN [$cTmp];");
            $stmt-&gt;execute();
        }

        return  $stmt;
    }

    public function close(){
        $this-&gt;db = null;
    }

    public function connect(){

        try {
            $this-&gt;db = new PDO ("dblib:host=$this-&gt;hostname:$this-&gt;port;dbname=$this-&gt;dbname", "$this-&gt;username", "$this-&gt;pwd");

           

        } catch (PDOException $e) {
            $this-&gt;logsys .= "Failed to get DB handle: " . $e-&gt;getMessage() . "\n";
        }

    }

}

?>
```
  

#

There is currently little sybase related PDO docs out there. The ones that I found often mention a spec for a dsn that is invalid. Here&apos;s how I am currently connecting to sybase ASE:<br><br>1. Compile up freetds http://www.freetds.org on top of open client;<br>2. Add the PDO and PD_DBLIB modules to php 5 as per the documentation; Note: I&apos;m currently using the PDO-beta and PDO_DBLIB-beta;<br>3. Check mods installed ok using "pear list" and "php -m";<br><br>The documentation often says to use "sybase:" as your DSN, this doesn&apos;t work. Use "dblib:" instead. Here&apos;s an example:<br><br>

```
<?php
  try {
    $hostname = "myhost";
    $port = 10060;
    $dbname = "tempdb";
    $username = "dbuser";
    $pw = "password";
    $dbh = new PDO ("dblib:host=$hostname:$port;dbname=$dbname","$username","$pw");
  } catch (PDOException $e) {
    echo "Failed to get DB handle: " . $e-&gt;getMessage() . "\n";
    exit;
  }
  $stmt = $dbh-&gt;prepare("select name from master..sysdatabases where name = db_name()");
  $stmt-&gt;execute();
  while ($row = $stmt-&gt;fetch()) {
    print_r($row);
  }
  unset($dbh); unset($stmt);
?>
```
<br><br>Hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-dblib.php)

**[To root](/README.md)**