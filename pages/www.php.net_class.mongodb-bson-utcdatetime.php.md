# The MongoDB\BSON\UTCDateTime class



When using MongoDB\BSON\UTCDateTime to query be sure for instances of strtotime you are putting the constructor in milliseconds:<br><br>$start = new MongoDB\BSON\UTCDateTime(strtotime("midnight") * 1000);<br>$filter = [&apos;date&apos; =&gt; [&apos;$gte&apos; =&gt; $start]];<br>$options = [&apos;sort&apos; =&gt; [&apos;date&apos; =&gt; -1]];<br>$query = new MongoDB\Driver\Query($filter,$options);  

---

[Official documentation page](https://www.php.net/manual/en/class.mongodb-bson-utcdatetime.php)

**[To root](/README.md)**