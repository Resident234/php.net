# Using the PHP Library for MongoDB (PHPLIB)





To test your connection string, you can do something like this:



```
<?php
$mongo = new MongoDB\Client(&apos;mongodb://my_server_does_not_exist_here:27017&apos;);
try 
{
&#xA0; &#xA0; $dbs = $mongo-&gt;listDatabases();
}
catch (MongoDB\Driver\Exception\ConnectionTimeoutException $e)
{
&#xA0; &#xA0; // PHP cannot find a MongoDB server using the MongoDB connection string specified
&#xA0; &#xA0; // do something here
}
?>
```



  

#



Well most of the tutorials didn&apos;t explained well, So i hope this might help someone 
Note: this is a part of my laravel project&#xA0; 

//getting data from a collection


```
<?php

use MongoDB\Client as Mongo;

$user = &quot;admin&quot;;
$pwd = &apos;password&apos;;

$mongo = new Mongo(&quot;mongodb://${user}:${pwd}@127.0.0.1:27017&quot;);
$collection = $mongo-&gt;db_name-&gt;collection;
$result = $collection-&gt;find()-&gt;toArray();

print_r($result);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.tutorial.library.php)

**[To root](/README.md)**