# PDO::setAttribute



Because no examples are provided, and to alleviate any confusion as a result, the setAttribute() method is invoked like so:<br><br>setAttribute(ATTRIBUTE, OPTION);<br><br>So, if I wanted to ensure that the column names returned from a query were returned in the case the database driver returned them (rather than having them returned in all upper case [as is the default on some of the PDO extensions]), I would do the following:<br><br>

```
<?php
// Create a new database connection.
$dbConnection = new PDO($dsn, $user, $pass);

// Set the case in which to return column_names.
$dbConnection->setAttribute(PDO::ATTR_CASE, PDO::CASE_NATURAL);
?>
```
<br><br>Hope this helps some of you who learn by example (as is the case with me).<br><br>.Colin  

---

This is an update to a note I wrote earlier concerning how to set multiple attributes when you create you PDO connection string.<br><br>You can put all the attributes you want in an associative array and pass that array as the fourth parameter in your connection string. So it goes like this: <br>

```
<?php
$options = [
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_CASE => PDO::CASE_NATURAL,
    PDO::ATTR_ORACLE_NULLS => PDO::NULL_EMPTY_STRING
];

// Now you create your connection string
try {
    // Then pass the options as the last parameter in the connection string
    $connection = new PDO("mysql:host=$host; dbname=$dbname", $user, $password, $options);

    // That's how you can set multiple attributes
} catch(PDOException $e) {
    die("Database connection failed: " . $e->getMessage());
}
?>
```
  

---

Well, I have not seen it mentioned anywhere and thought its worth mentioning. It might help someone. If you are wondering whether you can set multiple attributes then the answer is yes.<br><br>You can do it like this:<br>try {<br>    $connection = new PDO("mysql:host=$host; dbname=$dbname", $user, $password);<br>    // You can begin setting all the attributes you want.<br>    $connection-&gt;setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);<br>    $connection-&gt;setAttribute(PDO::ATTR_CASE, PDO::CASE_NATURAL);<br>    $connection-&gt;setAttribute(PDO::ATTR_ORACLE_NULLS, PDO::NULL_EMPTY_STRING);<br><br>    // That&apos;s how you can set multiple attributes<br>}<br>catch(PDOException $e)<br>{<br>    die("Database connection failed: " . $e-&gt;getMessage());<br>}<br><br>I hope this helps somebody. :)  

---

It is worth noting that not all attributes may be settable via setAttribute().  For example, PDO::MYSQL_ATTR_MAX_BUFFER_SIZE is only settable in PDO::__construct().  You must pass PDO::MYSQL_ATTR_MAX_BUFFER_SIZE as part of the optional 4th parameter to the constructor.  This is detailed in http://bugs.php.net/bug.php?id=38015  

---

There is also a way to specifie the default fetch mode :<br>

```
<?php
$connection = new PDO($connection_string);
$connection->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_OBJ);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/pdo.setattribute.php)

**[To root](/README.md)**