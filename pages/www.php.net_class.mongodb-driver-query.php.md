# The MongoDB\Driver\Query class



Find By _id <br><br>$mongo = new \MongoDB\Driver\Manager(&apos;mongodb://root:root@192.168.0.127/db&apos;);<br><br>$id           = new \MongoDB\BSON\ObjectId("588c78ce02ac660426003d87");<br>$filter      = [&apos;_id&apos; =&gt; $id];<br>$options = [];<br><br>$query = new \MongoDB\Driver\Query($filter, $options);<br>$rows   = $mongo-&gt;executeQuery(&apos;db.collectionName&apos;, $query); <br><br>foreach ($rows as $document) {<br>  pr($document);<br>}  

#

Here is an example of Query to retrieve the records from MangoDB collection using a filter. It will return in this case just one record that satisfy the filter id = 2.<br><br>Considering the following MangoDB collection:<br><br>

```
<?php
/* my_collection */

/* 1 */
{
    "_id" : ObjectId("5707f007639a94cbc600f282"),
    "id" : 1,
    "name" : "Name 1"
}

/* 2 */
{
    "_id" : ObjectId("5707f0a8639a94f4cd2c84b1"),
    "id" : 2,
    "name" : "Name 2"
}
?>
```


I'm using the following code:


```
<?php
$filter = ['id' => 2];
$options = [
   'projection' => ['_id' => 0],
];
$query = new MongoDB\Driver\Query($filter, $options);
$rows = $mongo->executeQuery('db_name.my_collection', $query); // $mongo contains the connection object to MongoDB
foreach($rows as $r){
   print_($r);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-query.php)

**[To root](/README.md)**