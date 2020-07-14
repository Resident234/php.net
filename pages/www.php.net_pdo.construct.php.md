# PDO::__construct



To get UTF-8 charset you can specify that in the DSN.<br><br>$link = new PDO("mysql:host=localhost;dbname=DB;charset=UTF8");  

#

I&apos;d like to point out that in PHP 7.0 in the dsn parameter you can&apos;t use &apos;host=localhost&apos; to solve this you can use &apos;host=127.0.0.1&apos; instead.  

#

To specify a database connection port use the following DSN string<br><br>

```
<?php
$dsn = &apos;mysql:dbname=testdb;host=127.0.0.1;port=3333&apos;;
?>
```
  

#

To connect throught unix socket you need to use <br>

```
<?php
$dsn = &apos;mysql:dbname=testdb;unix_socket=/path/to/socket&apos;;
?>
```
<br><br>You musn&apos;t specify host when using socket.  

#

If you use the UTF-8 encoding, you have to use the fourth parameter :<br><br>

```
<?php
$db = new PDO(&apos;mysql:host=myhost;dbname=mydb&apos;, &apos;login&apos;, &apos;password&apos;, array(PDO::MYSQL_ATTR_INIT_COMMAND =&gt; &apos;SET NAMES \&apos;UTF8\&apos;&apos;));
?>
```
  

#

Sqlite:<br><br>

```
<?php
try{     
    $pdo = new PDO(&apos;sqlite:example.db&apos;);
}catch (PDOException $e){
     die (&apos;DB Error&apos;);
}
?>
```
<br><br>If &apos;example.db&apos; does not exist, no exception is thrown but the file &apos;example.db&apos; is created.  

#

[Official documentation page](https://www.php.net/manual/en/pdo.construct.php)

**[To root](/README.md)**