# Using the PHP Library for MongoDB (PHPLIB)



To test your connection string, you can do something like this:<br><br>

```
<?php
$mongo = new MongoDB\Client('mongodb://my_server_does_not_exist_here:27017');
try 
{
    $dbs = $mongo->listDatabases();
}
catch (MongoDB\Driver\Exception\ConnectionTimeoutException $e)
{
    // PHP cannot find a MongoDB server using the MongoDB connection string specified
    // do something here
}
?>
```
  

#

Well most of the tutorials didn&apos;t explained well, So i hope this might help someone <br>Note: this is a part of my laravel project  <br><br>//getting data from a collection<br>

```
<?php

use MongoDB\Client as Mongo;

$user = "admin";
$pwd = 'password';

$mongo = new Mongo("mongodb://${user}:${pwd}@127.0.0.1:27017");
$collection = $mongo->db_name->collection;
$result = $collection->find()->toArray();

print_r($result);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.tutorial.library.php)

**[To root](/README.md)**