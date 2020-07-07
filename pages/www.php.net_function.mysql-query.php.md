# mysql_query





Simulating an atomic operation for application locks using mysql.

$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;pass&apos;);
if (!$link) {
&#xA0; &#xA0; die(&apos;Not connected : &apos; . mysql_error());
}

// make foo the current db
$db_selected = mysql_select_db(&apos;foo&apos;, $link);
if (!$db_selected) {
&#xA0; &#xA0; die (&apos;Can\&apos;t use foo : &apos; . mysql_error());
}

$q = &quot;update `table` set `LOCK`=&apos;F&apos; where `ID`=&apos;1&apos;&quot;;
$lock = mysql_affected_rows();

If we assume
&#xA0; &#xA0;&#xA0; NOT LOCKED = &quot;&quot; (empty string)
&#xA0; &#xA0;&#xA0; LOCKED = &apos;F&apos;

then if the column LOCK had a value other than F (normally should be an empty string) the update statement sets it to F and set the affected rows to 1. Which mean than we got the lock.
If affected rows return 0 then the value of that column was already F and somebody else has the lock.

The secret lies in the following statement taken from the mysql manual:
&quot;If you set a column to the value it currently has, MySQL notices this and does not update it.&quot;

Of course all this is possible if the all application processes agree on the locking algorithm.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-query.php)

**[To root](/README.md)**