# mysqli::set_charset





The comment by Claude (http://php.net/manual/en/mysqli.set-charset.php#121067) is CORRECT.

Setting the charset (it&apos;s really the encoding) like this after setting up your connection:
$connection-&gt;set_charset(&quot;utf8mb4&quot;)

FAILS to set the proper collation for the connection:

character_set_client: utf8mb4
character_set_connection: utf8mb4
character_set_database: utf8mb4
character_set_filesystem: binary
character_set_results: utf8mb4
character_set_server: utf8mb4
character_set_system: utf8
collation_connection: utf8mb4_general_ci &lt;---- still says general
collation_database: utf8mb4_unicode_ci
collation_server: utf8mb4_unicode_ci

If you use SET NAMES, that works:
$connection-&gt;query(&quot;SET NAMES utf8mb4 COLLATE utf8mb4_unicode_ci&quot;);

character_set_client: utf8mb4
character_set_connection: utf8mb4
character_set_database: utf8mb4
character_set_filesystem: binary
character_set_results: utf8mb4
character_set_server: utf8mb4
character_set_system: utf8
collation_connection: utf8mb4_unicode_ci &lt;-- now says unicode
collation_database: utf8mb4_unicode_ci
collation_server: utf8mb4_unicode_ci

Please note, that I set the following variables on the server:

Set the following to be: utf8mb4_unicode_ci

character_set_client
character_set_connection
character_set_database
character_set_results
character_set_server

collation_connection 
collation_server

Set:

character-set-client-handshake = FALSE or 0
skip-character-set-client-handshake = TRUE or 1

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.set-charset.php)

**[To root](/README.md)**