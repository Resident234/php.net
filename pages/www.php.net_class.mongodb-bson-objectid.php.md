# The MongoDB\BSON\ObjectId class




<div class="phpcode"><span class="html">
I struggled for awhile to identify the way to find() using a ObjectID <br><br>This seems to work, I hope this helps someone else out.&#xA0; <br><br>$mongoId = &apos;5a2493c33c95a1281836eb6a&apos;;<br><br>$collection-&gt;find([&apos;_id&apos;=&gt; new MongoDB\BSON\ObjectId(&quot;$mongoId&quot;)]);<br><br>I found it here:&#xA0;&#xA0; <a href="https://docs.mongodb.com/php-library/current/reference/method/MongoDBCollection-findOne/" rel="nofollow" target="_blank">https://docs.mongodb.com/php-library/current/reference/method/MongoDBCollection-findOne/</a><br><br>Note this is for the PHP library, not the legacy library.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-bson-objectid.php)

**[To root](/README.md)**