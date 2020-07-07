# The MongoDB\Driver\Query class





Find By _id 

$mongo = new \MongoDB\Driver\Manager(&apos;mongodb://root:root@192.168.0.127/db&apos;);

$id&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = new \MongoDB\BSON\ObjectId(&quot;588c78ce02ac660426003d87&quot;);
$filter&#xA0; &#xA0; &#xA0; = [&apos;_id&apos; =&gt; $id];
$options = [];

$query = new \MongoDB\Driver\Query($filter, $options);
$rows&#xA0;&#xA0; = $mongo-&gt;executeQuery(&apos;db.collectionName&apos;, $query); 

foreach ($rows as $document) {
&#xA0; pr($document);
}

  

#



Here is an example of Query to retrieve the records from MangoDB collection using a filter. It will return in this case just one record that satisfy the filter id = 2.

Considering the following MangoDB collection:



```
<?php
/* my_collection */

/* 1 */
{
&#xA0; &#xA0; &quot;_id&quot; : ObjectId(&quot;5707f007639a94cbc600f282&quot;),
&#xA0; &#xA0; &quot;id&quot; : 1,
&#xA0; &#xA0; &quot;name&quot; : &quot;Name 1&quot;
}

/* 2 */
{
&#xA0; &#xA0; &quot;_id&quot; : ObjectId(&quot;5707f0a8639a94f4cd2c84b1&quot;),
&#xA0; &#xA0; &quot;id&quot; : 2,
&#xA0; &#xA0; &quot;name&quot; : &quot;Name 2&quot;
}
?>
```


I&apos;m using the following code:


```
<?php
$filter = [&apos;id&apos; =&gt; 2];
$options = [
&#xA0;&#xA0; &apos;projection&apos; =&gt; [&apos;_id&apos; =&gt; 0],
];
$query = new MongoDB\Driver\Query($filter, $options);
$rows = $mongo-&gt;executeQuery(&apos;db_name.my_collection&apos;, $query); // $mongo contains the connection object to MongoDB
foreach($rows as $r){
&#xA0;&#xA0; print_($r);
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-query.php)

**[To root](/README.md)**