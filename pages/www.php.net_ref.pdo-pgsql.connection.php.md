# PDO_PGSQL DSN



You can also connect to PostgreSQL via a UNIX domain socket by leaving the host empty.  This should have less overhead than using TCP e.g.:<br><br>$dbh = new PDO(&apos;pgsql:user=exampleuser dbname=exampledb password=examplepass&apos;);<br><br>In fact as the C library call PQconnectdb underlies this implementation, you can supply anything that this library call would take - the "pgsql:" prefix gets stripped off before PQconnectdb is called, and if you supply any of the optional arguments (e.g. user), then these arguments will be added to the string that you supplied...  Check the docs for your relevant PostgreSQL client library: e.g.<br><br>http://www.postgresql.org/docs/8.3/static/libpq-connect.html<br><br>If you really want, you can use &apos;;&apos;s to separate your arguments - these will just be converted to spaces before PQconnectdb is called.<br><br>Tim.  

#

The PDO_PGSQL DSN should be seperated by semi-colons not spaces. It should follow the convention like the rest of the PDO DSNs.<br><br>&apos;pgsql:dbname=example;user=nobody;password=change_me;host=localhost&apos;  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-pgsql.connection.php)

**[To root](/README.md)**