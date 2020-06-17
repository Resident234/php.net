# mysql_query




<div class="phpcode"><span class="html">
Simulating an atomic operation for application locks using mysql.<br><br>$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;pass&apos;);<br>if (!$link) {<br>&#xA0; &#xA0; die(&apos;Not connected : &apos; . mysql_error());<br>}<br><br>// make foo the current db<br>$db_selected = mysql_select_db(&apos;foo&apos;, $link);<br>if (!$db_selected) {<br>&#xA0; &#xA0; die (&apos;Can\&apos;t use foo : &apos; . mysql_error());<br>}<br><br>$q = &quot;update `table` set `LOCK`=&apos;F&apos; where `ID`=&apos;1&apos;&quot;;<br>$lock = mysql_affected_rows();<br><br>If we assume<br>&#xA0; &#xA0;&#xA0; NOT LOCKED = &quot;&quot; (empty string)<br>&#xA0; &#xA0;&#xA0; LOCKED = &apos;F&apos;<br><br>then if the column LOCK had a value other than F (normally should be an empty string) the update statement sets it to F and set the affected rows to 1. Which mean than we got the lock.<br>If affected rows return 0 then the value of that column was already F and somebody else has the lock.<br><br>The secret lies in the following statement taken from the mysql manual:<br>&quot;If you set a column to the value it currently has, MySQL notices this and does not update it.&quot;<br><br>Of course all this is possible if the all application processes agree on the locking algorithm.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-query.php)

**[To root](/README.md)**