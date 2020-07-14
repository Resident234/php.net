# Persistent Database Connections



One additional not regarding odbc_pconnect  and possibly other variations of pconnect:<br> <br>If the connection encounters an error (bad SQL, incorrect request, etc), that error will return with  be present in odbc_errormsg for every subsequent action on that connection, even if subsequent actions don&apos;t cause another error.<br><br>For example:<br><br>A script connects with odbc_pconnect.<br>The connection is created on it&apos;s first use.<br>The script calls a query "Select * FROM Table1".<br>Table1 doesn&apos;t exist and odbc_errormsg contains that error.<br><br>Later(days, perhaps), a different script is called using the same parameters to odbc_pconnect.<br>The connection already exists, to it is reused.<br>The script calls a query "Select * FROM Table0".<br>The query runs fine, but odbc_errormsg still returns the error about Table1 not existing.<br><br>I&apos;m not seeing a way to clear that error using odbc_ functions, so keep your eyes open for this gotcha or use odbc_connect instead.  

#

[Official documentation page](https://www.php.net/manual/en/features.persistent-connections.php)

**[To root](/README.md)**