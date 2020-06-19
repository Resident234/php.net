# Installing/Configuring




<div class="phpcode"><span class="html">
This happened to me also with PHP 5.4.1<br>Copying the offending DLL everywhere didn&apos;t worket, and I don&apos;t have Postgres installed in the server, but also planned to use PHP against different Postgres versions, so the only solution I found that worked was to put in httpd.conf a line like this:<br><br>LoadFile &quot;C:/Program Files/PostgreSQL/8.4/bin/libpq.dll&quot; <br><br>but refering to the libpq.dll that comes bundled with PHP, like this:<br><br>LoadFile &quot;C:/php/libpq.dll&quot; <br><br>After that it worked fine to me.<br><br>(c) <a href="https://stackoverflow.com/users/1373641/dayron-armas-pe&#xF1;a" rel="nofollow" target="_blank">https://stackoverflow.com/users/1373641/dayron-armas-pe&#xF1;a</a><br>from: <a href="https://stackoverflow.com/questions/551734/php-not-loading-php-pgsql-dll-on-windows/10439560#10439560" rel="nofollow" target="_blank">https://stackoverflow.com/questions/551734/php-not-loading-php-pgsql-dll-on-windows/10439560#10439560</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pgsql.setup.php)

**[To root](/README.md)**