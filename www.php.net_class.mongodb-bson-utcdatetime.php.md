# The MongoDB\BSON\UTCDateTime class




<div class="phpcode"><span class="html">
When using MongoDB\BSON\UTCDateTime to query be sure for instances of strtotime you are putting the constructor in milliseconds:<br><br>$start = new MongoDB\BSON\UTCDateTime(strtotime(&quot;midnight&quot;) * 1000);<br>$filter = [&apos;date&apos; =&gt; [&apos;$gte&apos; =&gt; $start]];<br>$options = [&apos;sort&apos; =&gt; [&apos;date&apos; =&gt; -1]];<br>$query = new MongoDB\Driver\Query($filter,$options);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-bson-utcdatetime.php)

**[⬆ to root](/)**