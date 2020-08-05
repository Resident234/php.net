# Connections and Connection management



Using PHP 5.4.26, pdo_pgsql with libpg 9.2.8 (self compiled). As usual PHP never explains some critical stuff in documentation. You shouldn&apos;t expect that your connection is closed when you set $dbh = null unless all you do is just instantiating PDO class. Try following:<br><br>

```
<?php
$pdo = new PDO('pgsql:host=192.168.137.1;port=5432;dbname=anydb', 'anyuser', 'pw');
sleep(5);
$stmt = $pdo->prepare('SELECT * FROM sometable');
$stmt->execute();
$pdo = null;
sleep(60);
?>
```


Now check your database. And what a surprise! Your connection hangs for another 60 seconds. Now that might be expectable because you haven't cleared the resultset.



```
<?php
$pdo = new PDO('pgsql:host=192.168.137.160;port=5432;dbname=platin', 'cappytoi', '1111');
sleep(5);
$stmt = $pdo->prepare('SELECT * FROM admin');
$stmt->execute();
$stmt->closeCursor();
$pdo = null;
sleep(60);
?>
```


What teh heck you say at this point? Still same? Here is what you need to do to close that connection:



```
<?php
$pdo = new PDO('pgsql:host=192.168.137.160;port=5432;dbname=platin', 'cappytoi', '1111');
sleep(5);
$stmt = $pdo->prepare('SELECT * FROM admin');
$stmt->execute();
$stmt->closeCursor(); // this is not even required
$stmt = null; // doing this is mandatory for connection to get closed
$pdo = null;
sleep(60);
?>
```


PDO is just one of a kind because it saves you to depend on 3rd party abstraction layers. But it becomes annoying to see there is no implementation of a "disconnect" method even though there is a request for it for 2 years. Developers underestimate the requirement of such a method. First of all, doing $stmt = null  everywhere is annoying and what is most annoying is you cannot forcibly disconnect even when you set $pdo = null. It might get cleared on script's termination but this is not always possible because script termination may delayed due to slow client connection etc.

Anyway here is how to disconnect forcibly using postgresql:



```
<?php
$pdo = new PDO('pgsql:host=192.168.137.160;port=5432;dbname=platin', 'cappytoi', '1111');
sleep(5);
$stmt = $pdo->prepare('SELECT * FROM admin');
$stmt->execute();
$pdo->query('SELECT pg_terminate_backend(pg_backend_pid());');
$pdo = null;
sleep(60);
?>
```
<br><br>Following may be used for MYSQL: (not guaranteed)<br>KILL CONNECTION_ID()  

---

I would please advice people who talk about database port in reference with socket files to please read up about what a socket file is. TCP/IP uses ports, a socket file however is a direct pipe line to your database. So no, you should not replace localhost with local ip if you use a different port on your database server, because the socket file has nothing to do with your TCP/IP setup. And whenever possible, using the local socket file is much faster than establishing new TCP/IP connections on each request which is only meant for remote database servers.  

---

Just thought I&apos;d add in and give an explanation as to why you need to use 127.0.0.1 if you have a different port number.<br><br>The mysql libraries will automatically use Unix sockets if the host of "localhost" is used. To force TCP/IP you need to set an IP address.  

---

As http://stackoverflow.com/questions/17630772/pdo-cannot-connect-remote-mysql-server points out; sometimes when you want to connect to an external server like this:<br><br>

```
<?php
$conn = new PDO('mysql:host=123.4.5.6;dbname=test_db;port=3306','username','password');
?>
```


it will fail no matter what. However if you put a space between mysql: and host like this:



```
<?php
$conn = new PDO('mysql: host=123.4.5.6;dbname=test_db;port=3306','username','password');
?>
```
<br><br>it will magically work. I&apos;m not sure if this applies in all cases or server setups. But I think it&apos;s worth mentioning in the docs.  

---

To avoid exposing your connection details should you fail to remember to catch any exception thrown by the PDO constructor you can use the following class to implicitly change the exception handler temporarily.<br><br>

```
<?php

Class SafePDO extends PDO {
 
        public static function exception_handler($exception) {
            // Output the exception details
            die('Uncaught exception: ', $exception->getMessage());
        }
 
        public function __construct($dsn, $username='', $password='', $driver_options=array()) {

            // Temporarily change the PHP exception handler while we . . .
            set_exception_handler(array(__CLASS__, 'exception_handler'));

            // . . . create a PDO object
            parent::__construct($dsn, $username, $password, $driver_options);

            // Change the exception handler back to whatever it was before
            restore_exception_handler();
        }

}

// Connect to the database with defined constants
$dbh = new SafePDO(PDO_DSN, PDO_USER, PDO_PASSWORD);

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/pdo.connections.php)

**[To root](/README.md)**