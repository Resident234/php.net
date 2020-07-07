# PDO_MYSQL DSN





I have tested this and found that the &quot;dbname&quot; field is optional.&#xA0; Which is a good thing if you must first create the db.

After creating a db be sure to exec a &quot;use dbname;&quot;&#xA0; command, or else use fully specified table references.

  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-mysql.connection.php)

**[To root](/README.md)**