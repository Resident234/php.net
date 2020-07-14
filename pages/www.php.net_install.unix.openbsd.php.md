# OpenBSD installation notes



A brief update: As of OpenBSD 5.7 (2015) the installation process is extremely easy. Apache httpd was replaced by Nginx, which has since been further replaced by OpenBSD&apos;s own server, aptly named &apos;httpd&apos;. <br><br>&apos;httpd&apos; is installed by default, everything else you can still get from packages, with a couple name changes (including Apache and Nginx.) You will be asked which version to install - at the time of writing, versions 5.3.29p1 thru 5.6.5 are available.<br><br>#pkg_add php<br>#pkg_add ?>
```
fpm<br>#pkg_add pear<br><br>----<br>OpenBSD disables most services by default; a blank &apos;_flags&apos; line overrides default &apos;NO&apos; value. pkg_scripts are located in /etc/rc.d/<br>To start at boot, edit "/etc/rc.conf.local":<br><br>  httpd_flags=<br>  pkg_scripts=php_fpm<br><br>----<br>Example /etc/httpd.conf<br>#<br># paths are relative to chroot - e.g, &apos;/var/www/run/?>
```
fpm.sock&apos;<br>server "default" {<br>      listen on * port 80<br>      location "*.php" {<br>            fastcgi socket "/run/?>
```
fpm.sock"<br>      }<br>      directory index index.php<br>      root "/htdocs"<br>}<br><br>----<br>For date, timezone issues, copy /etc/localtime:<br>    $cp /etc/localtime /var/www/etc/localtime<br><br>If &apos;localhost&apos; DNS name fails to resolve, copy /etc/hosts<br>    $cp /etc/hosts /var/www/etc/hosts  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.openbsd.php)

**[To root](/README.md)**