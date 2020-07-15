# Microsoft SQL Server and Sybase Functions (PDO_DBLIB)



Hi All, I&apos;wrote a class to connect to MSSQL/Azure databases with Transaction support.<br><br>Hope this can help anyone!<br><br>

```
<?php
/**
*    @author     Johan Kasselman <johankasselman@live.com>
*    @since         2015-09-28    V1
*
*/

class pdo_dblib_mssql{

    private $db;
       private $cTransID;
       private $childTrans = array();

    public function __construct($hostname, $port, $dbname, $username, $pwd){

        $this->hostname = $hostname;
        $this->port = $port;
        $this->dbname = $dbname;
        $this->username = $username;
        $this->pwd = $pwd;

        $this->connect();
        
    }

    public function beginTransaction(){

        $cAlphanum = "AaBbCc0Dd1EeF2fG3gH4hI5iJ6jK7kLlM8mN9nOoPpQqRrSsTtUuVvWwXxYyZz";
        $this->cTransID = "T".substr(str_shuffle($cAlphanum), 0, 7);

        array_unshift($this->childTrans, $this->cTransID);

        $stmt = $this->db->prepare("BEGIN TRAN [$this->cTransID];");
        return $stmt->execute();

    }

    public function rollBack(){
        
        while(count($this->childTrans) > 0){
            $cTmp = array_shift($this->childTrans);
            $stmt = $this->db->prepare("ROLLBACK TRAN [$cTmp];");
            $stmt->execute();
        }

        return $stmt;
    }

    public function commit(){

        while(count($this->childTrans) > 0){
            $cTmp = array_shift($this->childTrans);
            $stmt = $this->db->prepare("COMMIT TRAN [$cTmp];");
            $stmt->execute();
        }

        return  $stmt;
    }

    public function close(){
        $this->db = null;
    }

    public function connect(){

        try {
            $this->db = new PDO ("dblib:host=$this->hostname:$this->port;dbname=$this->dbname", "$this->username", "$this->pwd");

           

        } catch (PDOException $e) {
            $this->logsys .= "Failed to get DB handle: " . $e->getMessage() . "\n";
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
    echo "Failed to get DB handle: " . $e->getMessage() . "\n";
    exit;
  }
  $stmt = $dbh->prepare("select name from master..sysdatabases where name = db_name()");
  $stmt->execute();
  while ($row = $stmt->fetch()) {
    print_r($row);
  }
  unset($dbh); unset($stmt);
?>
```
<br><br>Hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-dblib.php)

**[To root](/README.md)**