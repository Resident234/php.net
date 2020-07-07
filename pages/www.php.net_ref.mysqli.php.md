# Aliases and deprecated Mysqli Functions





Hints for upgrading PHP code from MySQL to MySQLi:

Note - MySQLi doesn&apos;t support persistent connection. If you need it, you must implement it by self. This case is not covered in this note.

First - change all occurences of MYSQL_ to MYSQLI_ and mysql_ to mysqli_

Most of changes is required because mysqli* functions has no implicit link argument, so it need to be added explicitly if not present already. Even if it is present, most mysqli_ functions require &apos;link&apos; argument as first (and mandatory) argument instead of last (and optional) as required by mysql_ functions. So, we need to change order of arguments at least.

So, you need to found names of all mysql_ functions used in your code, check if it need reoder of parameters or add the link parameter. Only *_connect() functions need review, but most scripts contain only few connect call.

If you use functions deprecated in mysql, then may not be impemented in mysqli. Those need to be reimplemented if required. In most case, it&apos;s very simple. In advance, PHP script written by carefull programer should not contain deprecated calls, so it&apos;s not problem in most cases at all.

Most of scripts I ported from mysql_ to mysqli_ can be converted by simple sed script followed by *_connect() function call review only. Especially when when programmer doesn&apos;t used the link as implicit argument and doesn&apos;t use deprecated functions the conversion of PHP source is trivial task and converted script work with no problem.

Special handling of some functions:

mysql_connect(),mysql_pconnect()
-----------------------------------------
Call with 3 or less parameters need not to be modified.
4-parameters call - delete 4th parameter as mysqli is always non-persistent
5-parameters - replace it by sequence of mysqli_init();mysqli_options();mysqli_real_connect()

mysql_create_db(), mysql_drop_db(), mysql_list_dbs(), mysql_db_name(),mysql_list_fields(), mysql_list_processes(), mysql_list_tables(), mysql_db_query(),mysql_table_name()
------------------------------------------
mysqli variant doesn&apos;t exist (those functions has been deprecated even in mysql_) but it&apos;s easy to reimplement each of it. Use apropropriate SQL command followed by standard loop for processing query results where applicable. If no connection to server yet you need to use explicit mysqli_connect call.

mysql_result()
-----------------
mysqli variant doesn&apos;t exist. Use one of mysqli_fetch_* variant (depending of type of mysql_result() third argument)

mysql_unbuffered_query()
-----------------
mysqli variant doesn&apos;t exist. Use mysqli_real_query()

  

#

[Official documentation page](https://www.php.net/manual/en/ref.mysqli.php)

**[To root](/README.md)**