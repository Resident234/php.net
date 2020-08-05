# MongoCollection::remove



To remove a document based on its ID, you need to ensure that you pass the ID as a MongoID object rather than just a string:<br><br>

```
<?php
$id = '4b3f272c8ead0eb19d000000';

// will not work:
$collection->remove(array('_id' => $id), true);

// will work:
$collection->remove(array('_id' => new MongoId($id)), true);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mongocollection.remove.php)

**[To root](/README.md)**