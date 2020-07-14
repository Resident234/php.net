# mysqli::set_charset



The comment by Claude (http://php.net/manual/en/mysqli.set-charset.php#121067) is CORRECT.<br><br>Setting the charset (it&apos;s really the encoding) like this after setting up your connection:<br>$connection-&gt;set_charset("utf8mb4")<br><br>FAILS to set the proper collation for the connection:<br><br>character_set_client: utf8mb4<br>character_set_connection: utf8mb4<br>character_set_database: utf8mb4<br>character_set_filesystem: binary<br>character_set_results: utf8mb4<br>character_set_server: utf8mb4<br>character_set_system: utf8<br>collation_connection: utf8mb4_general_ci &lt;---- still says general<br>collation_database: utf8mb4_unicode_ci<br>collation_server: utf8mb4_unicode_ci<br><br>If you use SET NAMES, that works:<br>$connection-&gt;query("SET NAMES utf8mb4 COLLATE utf8mb4_unicode_ci");<br><br>character_set_client: utf8mb4<br>character_set_connection: utf8mb4<br>character_set_database: utf8mb4<br>character_set_filesystem: binary<br>character_set_results: utf8mb4<br>character_set_server: utf8mb4<br>character_set_system: utf8<br>collation_connection: utf8mb4_unicode_ci &lt;-- now says unicode<br>collation_database: utf8mb4_unicode_ci<br>collation_server: utf8mb4_unicode_ci<br><br>Please note, that I set the following variables on the server:<br><br>Set the following to be: utf8mb4_unicode_ci<br><br>character_set_client<br>character_set_connection<br>character_set_database<br>character_set_results<br>character_set_server<br><br>collation_connection <br>collation_server<br><br>Set:<br><br>character-set-client-handshake = FALSE or 0<br>skip-character-set-client-handshake = TRUE or 1  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.set-charset.php)

**[To root](/README.md)**