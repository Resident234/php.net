# The MongoDB\BSON\ObjectId class





I struggled for awhile to identify the way to find() using a ObjectID 

This seems to work, I hope this helps someone else out.&#xA0; 

$mongoId = &apos;5a2493c33c95a1281836eb6a&apos;;

$collection-&gt;find([&apos;_id&apos;=&gt; new MongoDB\BSON\ObjectId(&quot;$mongoId&quot;)]);

I found it here:&#xA0;&#xA0; https://docs.mongodb.com/php-library/current/reference/method/MongoDBCollection-findOne/

Note this is for the PHP library, not the legacy library.

  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-bson-objectid.php)

**[To root](/README.md)**