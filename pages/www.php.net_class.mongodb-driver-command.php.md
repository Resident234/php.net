# The MongoDB\Driver\Command class





In case you&apos;re wondering how to perform a &apos;distinct&apos; query:



```
<?php

// Sample MongoDB command:
// db.product.distinct(&quot;scent&quot;, {&quot;prodCat&quot;: &quot;10 oz can&quot;})

$manager = new MongoDB\Driver\Manager(&quot;mongodb://localhost:27017&quot;);

$query = [&apos;prodCat&apos; =&gt; &apos;10 oz can&apos;]; // your typical MongoDB query
$cmd = new MongoDB\Driver\Command([
&#xA0; &#xA0; // build the &apos;distinct&apos; command
&#xA0; &#xA0; &apos;distinct&apos; =&gt; &apos;product&apos;, // specify the collection name
&#xA0; &#xA0; &apos;key&apos; =&gt; &apos;scent&apos;, // specify the field for which we want to get the distinct values
&#xA0; &#xA0; &apos;query&apos; =&gt; $query // criteria to filter documents
]);
$cursor = $manager-&gt;executeCommand(&apos;catalog&apos;, $cmd); // retrieve the results
$scents = current($cursor-&gt;toArray())-&gt;values; // get the distinct values as an array

var_dump($scents);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-command.php)

**[To root](/README.md)**