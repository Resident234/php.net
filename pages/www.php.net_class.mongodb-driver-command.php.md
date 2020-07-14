# The MongoDB\Driver\Command class



In case you&apos;re wondering how to perform a &apos;distinct&apos; query:<br><br>

```
<?php

// Sample MongoDB command:
// db.product.distinct("scent", {"prodCat": "10 oz can"})

$manager = new MongoDB\Driver\Manager("mongodb://localhost:27017");

$query = [&apos;prodCat&apos; =&gt; &apos;10 oz can&apos;]; // your typical MongoDB query
$cmd = new MongoDB\Driver\Command([
    // build the &apos;distinct&apos; command
    &apos;distinct&apos; =&gt; &apos;product&apos;, // specify the collection name
    &apos;key&apos; =&gt; &apos;scent&apos;, // specify the field for which we want to get the distinct values
    &apos;query&apos; =&gt; $query // criteria to filter documents
]);
$cursor = $manager-&gt;executeCommand(&apos;catalog&apos;, $cmd); // retrieve the results
$scents = current($cursor-&gt;toArray())-&gt;values; // get the distinct values as an array

var_dump($scents);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-command.php)

**[To root](/README.md)**