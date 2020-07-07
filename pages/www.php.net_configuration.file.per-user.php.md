# .user.ini files





If you have no idea what &quot;PHP_INI_PERDIR&quot; or &quot;PHP_INI_USER&quot; are or how they relate to setting a .user.ini file, take a look at the ini.list page: http://www.php.net/manual/en/ini.list.php

Basically, anything in the &quot;Changeable&quot; column labeled as PHP_INI_SYSTEM can&apos;t be set in the .user.ini file (so quit trying).&#xA0; It can ONLY be set at the main php.ini level.

  

#



&quot;If you are using Apache, use .htaccess files for the same effect.&quot;

To clarify, this applies only to Apache module mode. If you put php directives in .htaccess on an Apache CGI/FastCGI server, this will bomb the server out with a 500 error. Thus, you unfortunately cannot create a config which caters for both types of hosting, at least not in any straightforward way.

  

#

[Official documentation page](https://www.php.net/manual/en/configuration.file.per-user.php)

**[To root](/README.md)**