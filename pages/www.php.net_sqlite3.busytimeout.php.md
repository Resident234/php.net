# SQLite3::busyTimeout





The busyTimeout() method and related API sqlite3_busy_timeout() is a connection level attribute and affects whole connection and should be set once after opening connection.&#xA0; Do not set to zero or you will encounter &quot;Database is busy&quot; error message when calling query, querySingle, prepare, or execute methods.&#xA0; Also ensure that sqlite3 library is compiled with HAVE_USLEEP defined, otherwise busyTimeout() can only time out in seconds.&#xA0; It is very highly recommended to call busyTimeout() with non-zero timeout for reliability in concurrent environment.

  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3.busytimeout.php)

**[To root](/README.md)**