# mysql_query



Simulating an atomic operation for application locks using mysql.<br><br>$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;pass&apos;);<br>if (!$link) {<br>    die(&apos;Not connected : &apos; . mysql_error());<br>}<br><br>// make foo the current db<br>$db_selected = mysql_select_db(&apos;foo&apos;, $link);<br>if (!$db_selected) {<br>    die (&apos;Can\&apos;t use foo : &apos; . mysql_error());<br>}<br><br>$q = "update `table` set `LOCK`=&apos;F&apos; where `ID`=&apos;1&apos;";<br>$lock = mysql_affected_rows();<br><br>If we assume<br>     NOT LOCKED = "" (empty string)<br>     LOCKED = &apos;F&apos;<br><br>then if the column LOCK had a value other than F (normally should be an empty string) the update statement sets it to F and set the affected rows to 1. Which mean than we got the lock.<br>If affected rows return 0 then the value of that column was already F and somebody else has the lock.<br><br>The secret lies in the following statement taken from the mysql manual:<br>"If you set a column to the value it currently has, MySQL notices this and does not update it."<br><br>Of course all this is possible if the all application processes agree on the locking algorithm.  

---

[Official documentation page](https://www.php.net/manual/en/function.mysql-query.php)

**[To root](/README.md)**