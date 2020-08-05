# SQLite3::busyTimeout



The busyTimeout() method and related API sqlite3_busy_timeout() is a connection level attribute and affects whole connection and should be set once after opening connection.  Do not set to zero or you will encounter "Database is busy" error message when calling query, querySingle, prepare, or execute methods.  Also ensure that sqlite3 library is compiled with HAVE_USLEEP defined, otherwise busyTimeout() can only time out in seconds.  It is very highly recommended to call busyTimeout() with non-zero timeout for reliability in concurrent environment.  

---

[Official documentation page](https://www.php.net/manual/en/sqlite3.busytimeout.php)

**[To root](/README.md)**