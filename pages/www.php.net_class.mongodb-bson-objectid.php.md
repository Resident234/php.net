# The MongoDB\BSON\ObjectId class



I struggled for awhile to identify the way to find() using a ObjectID <br><br>This seems to work, I hope this helps someone else out.  <br><br>$mongoId = &apos;5a2493c33c95a1281836eb6a&apos;;<br><br>$collection-&gt;find([&apos;_id&apos;=&gt; new MongoDB\BSON\ObjectId("$mongoId")]);<br><br>I found it here:   https://docs.mongodb.com/?>
```
library/current/reference/method/MongoDBCollection-findOne/<br><br>Note this is for the PHP library, not the legacy library.  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-bson-objectid.php)

**[To root](/README.md)**