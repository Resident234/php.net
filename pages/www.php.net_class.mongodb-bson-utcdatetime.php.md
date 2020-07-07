# The MongoDB\BSON\UTCDateTime class





When using MongoDB\BSON\UTCDateTime to query be sure for instances of strtotime you are putting the constructor in milliseconds:

$start = new MongoDB\BSON\UTCDateTime(strtotime(&quot;midnight&quot;) * 1000);
$filter = [&apos;date&apos; =&gt; [&apos;$gte&apos; =&gt; $start]];
$options = [&apos;sort&apos; =&gt; [&apos;date&apos; =&gt; -1]];
$query = new MongoDB\Driver\Query($filter,$options);

  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-bson-utcdatetime.php)

**[To root](/README.md)**