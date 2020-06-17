# PDO::errorInfo




<div class="phpcode"><span class="html">
Please note : that this example won&apos;t work if PDO::ATTR_EMULATE_PREPARES is true. 
<br>
<br>You should set it to false
<br>
<br><span class="default">&lt;?php
<br>$dbh</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_EMULATE_PREPARES</span><span class="keyword">,</span><span class="default">false</span><span class="keyword">);
<br></span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;bogus sql&apos;</span><span class="keyword">);
<br>if (!</span><span class="default">$stmt</span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;\nPDO::errorInfo():\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">errorInfo</span><span class="keyword">());
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
here are the error codes for sqlite, straight from their site:
<br>
<br>The error codes for SQLite version 3 are unchanged from version 2. They are as follows: 
<br>#define SQLITE_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 0&#xA0;&#xA0; /* Successful result */
<br>#define SQLITE_ERROR&#xA0; &#xA0; &#xA0; &#xA0; 1&#xA0;&#xA0; /* SQL error or missing database */
<br>#define SQLITE_INTERNAL&#xA0; &#xA0;&#xA0; 2&#xA0;&#xA0; /* An internal logic error in SQLite */
<br>#define SQLITE_PERM&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 3&#xA0;&#xA0; /* Access permission denied */
<br>#define SQLITE_ABORT&#xA0; &#xA0; &#xA0; &#xA0; 4&#xA0;&#xA0; /* Callback routine requested an abort */
<br>#define SQLITE_BUSY&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 5&#xA0;&#xA0; /* The database file is locked */
<br>#define SQLITE_LOCKED&#xA0; &#xA0; &#xA0;&#xA0; 6&#xA0;&#xA0; /* A table in the database is locked */
<br>#define SQLITE_NOMEM&#xA0; &#xA0; &#xA0; &#xA0; 7&#xA0;&#xA0; /* A malloc() failed */
<br>#define SQLITE_READONLY&#xA0; &#xA0;&#xA0; 8&#xA0;&#xA0; /* Attempt to write a readonly database */
<br>#define SQLITE_INTERRUPT&#xA0; &#xA0; 9&#xA0;&#xA0; /* Operation terminated by sqlite_interrupt() */
<br>#define SQLITE_IOERR&#xA0; &#xA0; &#xA0;&#xA0; 10&#xA0;&#xA0; /* Some kind of disk I/O error occurred */
<br>#define SQLITE_CORRUPT&#xA0; &#xA0;&#xA0; 11&#xA0;&#xA0; /* The database disk image is malformed */
<br>#define SQLITE_NOTFOUND&#xA0; &#xA0; 12&#xA0;&#xA0; /* (Internal Only) Table or record not found */
<br>#define SQLITE_FULL&#xA0; &#xA0; &#xA0; &#xA0; 13&#xA0;&#xA0; /* Insertion failed because database is full */
<br>#define SQLITE_CANTOPEN&#xA0; &#xA0; 14&#xA0;&#xA0; /* Unable to open the database file */
<br>#define SQLITE_PROTOCOL&#xA0; &#xA0; 15&#xA0;&#xA0; /* Database lock protocol error */
<br>#define SQLITE_EMPTY&#xA0; &#xA0; &#xA0;&#xA0; 16&#xA0;&#xA0; /* (Internal Only) Database table is empty */
<br>#define SQLITE_SCHEMA&#xA0; &#xA0; &#xA0; 17&#xA0;&#xA0; /* The database schema changed */
<br>#define SQLITE_TOOBIG&#xA0; &#xA0; &#xA0; 18&#xA0;&#xA0; /* Too much data for one row of a table */
<br>#define SQLITE_CONSTRAINT&#xA0; 19&#xA0;&#xA0; /* Abort due to contraint violation */
<br>#define SQLITE_MISMATCH&#xA0; &#xA0; 20&#xA0;&#xA0; /* Data type mismatch */
<br>#define SQLITE_MISUSE&#xA0; &#xA0; &#xA0; 21&#xA0;&#xA0; /* Library used incorrectly */
<br>#define SQLITE_NOLFS&#xA0; &#xA0; &#xA0;&#xA0; 22&#xA0;&#xA0; /* Uses OS features not supported on host */
<br>#define SQLITE_AUTH&#xA0; &#xA0; &#xA0; &#xA0; 23&#xA0;&#xA0; /* Authorization denied */
<br>#define SQLITE_ROW&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 100&#xA0; /* sqlite_step() has another row ready */
<br>#define SQLITE_DONE&#xA0; &#xA0; &#xA0; &#xA0; 101&#xA0; /* sqlite_step() has finished executing */</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some PDO drivers return a larger array. For example, the SQL Server driver returns 5 values.
<br>
<br>For example:
<br><span class="default">&lt;?php
<br>$numRows </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">exec</span><span class="keyword">(</span><span class="string">&quot;DELETE FROM [TableName] WHERE ID between 6 and 17&quot;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">errorInfo</span><span class="keyword">());
<br></span><span class="default">?&gt;
<br></span>
<br>Result:
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; 00000
<br>&#xA0; &#xA0; [1] =&gt; 0
<br>&#xA0; &#xA0; [2] =&gt; (null) [0] (severity 0) []
<br>&#xA0; &#xA0; [3] =&gt; 0
<br>&#xA0; &#xA0; [4] =&gt; 0
<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.errorinfo.php)

**[To root](/README.md)**