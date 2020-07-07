# Persistent Database Connections





One additional not regarding odbc_pconnect&#xA0; and possibly other variations of pconnect:
 
If the connection encounters an error (bad SQL, incorrect request, etc), that error will return with&#xA0; be present in odbc_errormsg for every subsequent action on that connection, even if subsequent actions don&apos;t cause another error.

For example:

A script connects with odbc_pconnect.
The connection is created on it&apos;s first use.
The script calls a query &quot;Select * FROM Table1&quot;.
Table1 doesn&apos;t exist and odbc_errormsg contains that error.

Later(days, perhaps), a different script is called using the same parameters to odbc_pconnect.
The connection already exists, to it is reused.
The script calls a query &quot;Select * FROM Table0&quot;.
The query runs fine, but odbc_errormsg still returns the error about Table1 not existing.

I&apos;m not seeing a way to clear that error using odbc_ functions, so keep your eyes open for this gotcha or use odbc_connect instead.

  

#

[Official documentation page](https://www.php.net/manual/en/features.persistent-connections.php)

**[To root](/README.md)**