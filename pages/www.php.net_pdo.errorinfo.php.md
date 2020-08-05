# PDO::errorInfo



Please note : that this example won&apos;t work if PDO::ATTR_EMULATE_PREPARES is true. <br><br>You should set it to false<br><br>

```
<?php
$dbh->setAttribute(PDO::ATTR_EMULATE_PREPARES,false);
$stmt = $dbh->prepare('bogus sql');
if (!$stmt) {
    echo "\nPDO::errorInfo():\n";
    print_r($dbh->errorInfo());
}
?>
```
  

---

here are the error codes for sqlite, straight from their site:<br><br>The error codes for SQLite version 3 are unchanged from version 2. They are as follows: <br>#define SQLITE_OK           0   /* Successful result */<br>#define SQLITE_ERROR        1   /* SQL error or missing database */<br>#define SQLITE_INTERNAL     2   /* An internal logic error in SQLite */<br>#define SQLITE_PERM         3   /* Access permission denied */<br>#define SQLITE_ABORT        4   /* Callback routine requested an abort */<br>#define SQLITE_BUSY         5   /* The database file is locked */<br>#define SQLITE_LOCKED       6   /* A table in the database is locked */<br>#define SQLITE_NOMEM        7   /* A malloc() failed */<br>#define SQLITE_READONLY     8   /* Attempt to write a readonly database */<br>#define SQLITE_INTERRUPT    9   /* Operation terminated by sqlite_interrupt() */<br>#define SQLITE_IOERR       10   /* Some kind of disk I/O error occurred */<br>#define SQLITE_CORRUPT     11   /* The database disk image is malformed */<br>#define SQLITE_NOTFOUND    12   /* (Internal Only) Table or record not found */<br>#define SQLITE_FULL        13   /* Insertion failed because database is full */<br>#define SQLITE_CANTOPEN    14   /* Unable to open the database file */<br>#define SQLITE_PROTOCOL    15   /* Database lock protocol error */<br>#define SQLITE_EMPTY       16   /* (Internal Only) Database table is empty */<br>#define SQLITE_SCHEMA      17   /* The database schema changed */<br>#define SQLITE_TOOBIG      18   /* Too much data for one row of a table */<br>#define SQLITE_CONSTRAINT  19   /* Abort due to contraint violation */<br>#define SQLITE_MISMATCH    20   /* Data type mismatch */<br>#define SQLITE_MISUSE      21   /* Library used incorrectly */<br>#define SQLITE_NOLFS       22   /* Uses OS features not supported on host */<br>#define SQLITE_AUTH        23   /* Authorization denied */<br>#define SQLITE_ROW         100  /* sqlite_step() has another row ready */<br>#define SQLITE_DONE        101  /* sqlite_step() has finished executing */  

---

Some PDO drivers return a larger array. For example, the SQL Server driver returns 5 values.<br><br>For example:<br>

```
<?php
$numRows = $db->exec("DELETE FROM [TableName] WHERE ID between 6 and 17");
print_r($db->errorInfo());
?>
```
<br><br>Result:<br><br>Array<br>(<br>    [0] =&gt; 00000<br>    [1] =&gt; 0<br>    [2] =&gt; (null) [0] (severity 0) []<br>    [3] =&gt; 0<br>    [4] =&gt; 0<br>)  

---

[Official documentation page](https://www.php.net/manual/en/pdo.errorinfo.php)

**[To root](/README.md)**