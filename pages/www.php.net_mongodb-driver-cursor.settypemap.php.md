# MongoDB\Driver\Cursor::setTypeMap





If you would prefer to have the results returned as an associative array, after executing your query you could call $cursor-&gt;setTypeMap like this:

$cursor-&gt;setTypeMap([&apos;root&apos; =&gt; &apos;array&apos;, &apos;document&apos; =&gt; &apos;array&apos;, &apos;array&apos; =&gt; &apos;array&apos;]);

  

#

[Official documentation page](https://www.php.net/manual/en/mongodb-driver-cursor.settypemap.php)

**[To root](/README.md)**