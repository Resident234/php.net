# mssql_connect



If someone encounters the interesting problem in which PHP can connect to a MSSQL server from the command line but not when running as an Apache module: SELinux prevents Apache (and therefore all Apache modules) from making remote connections by default.<br><br>This solved the problem in CentOS:<br># setsebool -P httpd_can_network_connect=1  

#

[Official documentation page](https://www.php.net/manual/en/function.mssql-connect.php)

**[To root](/README.md)**