# SQLite3::exec



I was getting "database locked" all the time until I found out some features of sqlite3 must be set by using SQL special instructions (i.e. using PRAGMA keyword). For instance, what apparently solved my problem with "database locked" was to set journal_mode to &apos;wal&apos; (it is defaulting to &apos;delete&apos;, as stated here: https://www.sqlite.org/wal.html (see Activating  And Configuring WAL Mode)).<br><br>So basically what I had to do was creating a connection to the database and setting journal_mode with the SQL statement. Example:<br><br>

```
<?php
$db = new SQLite3('/my/sqlite/file.sqlite3');
$db->busyTimeout(5000);
// WAL mode has better control over concurrency.
// Source: https://www.sqlite.org/wal.html
$db->exec('PRAGMA journal_mode = wal;');
?>
```
<br><br>Hope that helps.  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3.exec.php)

**[To root](/README.md)**