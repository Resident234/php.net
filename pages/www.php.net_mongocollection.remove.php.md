# MongoCollection::remove



To remove a document based on its ID, you need to ensure that you pass the ID as a MongoID object rather than just a string:<br><br>

```
<?php
$id = &apos;4b3f272c8ead0eb19d000000&apos;;

// will not work:
$collection-&gt;remove(array(&apos;_id&apos; =&gt; $id), true);

// will work:
$collection-&gt;remove(array(&apos;_id&apos; =&gt; new MongoId($id)), true);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mongocollection.remove.php)

**[To root](/README.md)**