# ODBC and DB2 Functions (PDO_ODBC)



I just spent a couple of hours trying to track down the Exception "Could not find driver". This was despite having ODBC and PDO_ODBC installed, and all of the configuration seemed to be correct.<br><br>Turned out the problem was that I used ODBC in upper-case in the dsn. As soon as I changed the dns to "odbc:database" it worked.<br><br>As this code used to work a few months ago, this sudden case-sensitivity threw me for a loop. So in case you get this error, check the casing first.  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-odbc.php)

**[To root](/README.md)**