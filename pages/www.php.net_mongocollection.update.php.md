# MongoCollection::update



For anyone referencing records by the Mongo _id object, it&apos;s important to recognise that it is in fact an object, and not a string. <br><br>If you have a record with a Mongo ID of say "4e519d5118617e88f27ea8cd" that you are trying to retrieve or update, you cannot search for it using something like:<br>

```
<?php
$m = new Mongo();
$db = $m->selectDB('db');
$collection = 'collection';
$db->$collection->findOne(array('_id', '4e519d5118617e88f27ea8cd'));
?>
```


There is some documentation that mentions simple conversion to string will solve this, but I have found the only reliable way to locate records based on their ID is to first pass it to MondoID(), then use that for reference.

Something like this will be far more reliable:


```
<?php
$m = new Mongo();
$db = $m->selectDB('db');
$collection = 'collection';
$mongoID = new MongoID('4e519d5118617e88f27ea8cd');
$db->$collection->findOne(array('_id', $mongoID));
?>
```
<br><br>This may prove useful for anyone using the ID object like an auto-increment database key would be used in MySQL or similar.  

#

Please note under optional third parameter "options":<br><br>While the official MongoDB documentation references the keyword "multi" to flag the use of multiple updates, the PHP implementation uses the key "multiple" instead.<br><br>This may cause a little confusion if you&apos;re basing your keys on the OFFICIAL MongoDB documentation.  

#

[Official documentation page](https://www.php.net/manual/en/mongocollection.update.php)

**[To root](/README.md)**