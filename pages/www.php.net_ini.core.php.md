# Description of core php.ini directives



Starting with PHP 4.4.0 (at least PHP version 4.3.10 did have old, documented behaviour) interpretation of value of "session.save_path" did change in conjunction with "save_mode" and "open_basedir" enabled.<br><br>Documented ( http://de.php.net/manual/en/ref.session.php#ini.session.save-path ):<br>  Values of "session.save_path" should or may be  **without**  ending slash.<br>  For instance:<br>

```
<?php
  // Valid only  *before* PHP 4.4.0:
  ini_set( "session.save_path", "/var/httpd/kunde/phptmp" );
?>
```
 will mean:
  The directory "/var/httpd/kunde/phptmp/" will be used to write data and therefore must be writable by the web server.

Starting with PHP 4.4.0 the server complains that "/var/httpd/kunde/" is not writable.
Solution: Add an ending slash in call of ini_set (or probably whereever you set "session.save_path"), e.g.:


```
<?php
  // Note the slash on ".....phptmp/":
  ini_set( "session.save_path", "/var/httpd/kunde/phptmp/" );
?>
```
<br><br>Hope, that does help someone.  

#

[Official documentation page](https://www.php.net/manual/en/ini.core.php)

**[To root](/README.md)**