# PDO::errorInfo





Please note : that this example won&apos;t work if PDO::ATTR_EMULATE_PREPARES is true. 



You should set it to false





```
<?php

$dbh-&gt;setAttribute(PDO::ATTR_EMULATE_PREPARES,false);

$stmt = $dbh-&gt;prepare(&apos;bogus sql&apos;);

if (!$stmt) {

&#xA0; &#xA0; echo &quot;\nPDO::errorInfo():\n&quot;;

&#xA0; &#xA0; print_r($dbh-&gt;errorInfo());

}

?>
```



  

#



here are the error codes for sqlite, straight from their site:



The error codes for SQLite version 3 are unchanged from version 2. They are as follows: 

#define SQLITE_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 0&#xA0;&#xA0; /* Successful result */

#define SQLITE_ERROR&#xA0; &#xA0; &#xA0; &#xA0; 1&#xA0;&#xA0; /* SQL error or missing database */

#define SQLITE_INTERNAL&#xA0; &#xA0;&#xA0; 2&#xA0;&#xA0; /* An internal logic error in SQLite */

#define SQLITE_PERM&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 3&#xA0;&#xA0; /* Access permission denied */

#define SQLITE_ABORT&#xA0; &#xA0; &#xA0; &#xA0; 4&#xA0;&#xA0; /* Callback routine requested an abort */

#define SQLITE_BUSY&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 5&#xA0;&#xA0; /* The database file is locked */

#define SQLITE_LOCKED&#xA0; &#xA0; &#xA0;&#xA0; 6&#xA0;&#xA0; /* A table in the database is locked */

#define SQLITE_NOMEM&#xA0; &#xA0; &#xA0; &#xA0; 7&#xA0;&#xA0; /* A malloc() failed */

#define SQLITE_READONLY&#xA0; &#xA0;&#xA0; 8&#xA0;&#xA0; /* Attempt to write a readonly database */

#define SQLITE_INTERRUPT&#xA0; &#xA0; 9&#xA0;&#xA0; /* Operation terminated by sqlite_interrupt() */

#define SQLITE_IOERR&#xA0; &#xA0; &#xA0;&#xA0; 10&#xA0;&#xA0; /* Some kind of disk I/O error occurred */

#define SQLITE_CORRUPT&#xA0; &#xA0;&#xA0; 11&#xA0;&#xA0; /* The database disk image is malformed */

#define SQLITE_NOTFOUND&#xA0; &#xA0; 12&#xA0;&#xA0; /* (Internal Only) Table or record not found */

#define SQLITE_FULL&#xA0; &#xA0; &#xA0; &#xA0; 13&#xA0;&#xA0; /* Insertion failed because database is full */

#define SQLITE_CANTOPEN&#xA0; &#xA0; 14&#xA0;&#xA0; /* Unable to open the database file */

#define SQLITE_PROTOCOL&#xA0; &#xA0; 15&#xA0;&#xA0; /* Database lock protocol error */

#define SQLITE_EMPTY&#xA0; &#xA0; &#xA0;&#xA0; 16&#xA0;&#xA0; /* (Internal Only) Database table is empty */

#define SQLITE_SCHEMA&#xA0; &#xA0; &#xA0; 17&#xA0;&#xA0; /* The database schema changed */

#define SQLITE_TOOBIG&#xA0; &#xA0; &#xA0; 18&#xA0;&#xA0; /* Too much data for one row of a table */

#define SQLITE_CONSTRAINT&#xA0; 19&#xA0;&#xA0; /* Abort due to contraint violation */

#define SQLITE_MISMATCH&#xA0; &#xA0; 20&#xA0;&#xA0; /* Data type mismatch */

#define SQLITE_MISUSE&#xA0; &#xA0; &#xA0; 21&#xA0;&#xA0; /* Library used incorrectly */

#define SQLITE_NOLFS&#xA0; &#xA0; &#xA0;&#xA0; 22&#xA0;&#xA0; /* Uses OS features not supported on host */

#define SQLITE_AUTH&#xA0; &#xA0; &#xA0; &#xA0; 23&#xA0;&#xA0; /* Authorization denied */

#define SQLITE_ROW&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 100&#xA0; /* sqlite_step() has another row ready */

#define SQLITE_DONE&#xA0; &#xA0; &#xA0; &#xA0; 101&#xA0; /* sqlite_step() has finished executing */

  

#



Some PDO drivers return a larger array. For example, the SQL Server driver returns 5 values.



For example:



```
<?php

$numRows = $db-&gt;exec(&quot;DELETE FROM [TableName] WHERE ID between 6 and 17&quot;);

print_r($db-&gt;errorInfo());

?>
```




Result:



Array

(

&#xA0; &#xA0; [0] =&gt; 00000

&#xA0; &#xA0; [1] =&gt; 0

&#xA0; &#xA0; [2] =&gt; (null) [0] (severity 0) []

&#xA0; &#xA0; [3] =&gt; 0

&#xA0; &#xA0; [4] =&gt; 0

)

  

#

[Official documentation page](https://www.php.net/manual/en/pdo.errorinfo.php)

**[To root](/README.md)**