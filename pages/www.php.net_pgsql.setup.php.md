# Installing/Configuring



This happened to me also with PHP 5.4.1<br>Copying the offending DLL everywhere didn&apos;t worket, and I don&apos;t have Postgres installed in the server, but also planned to use PHP against different Postgres versions, so the only solution I found that worked was to put in httpd.conf a line like this:<br><br>LoadFile "C:/Program Files/PostgreSQL/8.4/bin/libpq.dll" <br><br>but refering to the libpq.dll that comes bundled with PHP, like this:<br><br>LoadFile "C:/php/libpq.dll" <br><br>After that it worked fine to me.<br><br>(c) https://stackoverflow.com/users/1373641/dayron-armas-pe&#xF1;a<br>from: https://stackoverflow.com/questions/551734/?>
```
not-loading

```
<??>
```
pgsql-dll-on-windows/10439560#10439560  

#

[Official documentation page](https://www.php.net/manual/en/pgsql.setup.php)

**[To root](/README.md)**