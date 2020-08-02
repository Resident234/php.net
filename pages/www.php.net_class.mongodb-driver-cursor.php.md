# The MongoDB\Driver\Cursor class



As one might notice, this class does not implement a hasNext() or next() method as opposed to the now deprecated Mongo driver.<br><br>If, for any reason, you need to pull data from the cursor procedurally or otherwise need to override the behavior of foreach while iterating on the cursor, the SPL \IteratorIterator class can be used. When doing so, it is important to rewind the iterator before using it, otherwise you won&apos;t get any data back.<br><br>

```
<?php
$cursor = $collection->find();
$it = new \IteratorIterator($cursor);
$it->rewind(); // Very important

while($doc = $it->current()) {
    var_dump($doc);
    $it->next();
}
?>
```
<br><br>I used this trick to build a backward compatibility wrapper emulating the old Mongo driver in order to upgrade an older codebase.  

---

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-cursor.php)

**[To root](/README.md)**