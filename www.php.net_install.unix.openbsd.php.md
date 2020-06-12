# OpenBSD installation notes




<div class="phpcode"><span class="html">
A brief update: As of OpenBSD 5.7 (2015) the installation process is extremely easy. Apache httpd was replaced by Nginx, which has since been further replaced by OpenBSD&apos;s own server, aptly named &apos;httpd&apos;. <br><br>&apos;httpd&apos; is installed by default, everything else you can still get from packages, with a couple name changes (including Apache and Nginx.) You will be asked which version to install - at the time of writing, versions 5.3.29p1 thru 5.6.5 are available.<br><br>#pkg_add php<br>#pkg_add php-fpm<br>#pkg_add pear<br><br>----<br>OpenBSD disables most services by default; a blank &apos;_flags&apos; line overrides default &apos;NO&apos; value. pkg_scripts are located in /etc/rc.d/<br>To start at boot, edit &quot;/etc/rc.conf.local&quot;:<br><br>&#xA0; httpd_flags=<br>&#xA0; pkg_scripts=php_fpm<br><br>----<br>Example /etc/httpd.conf<br>#<br># paths are relative to chroot - e.g, &apos;/var/www/run/php-fpm.sock&apos;<br>server &quot;default&quot; {<br>&#xA0; &#xA0; &#xA0; listen on * port 80<br>&#xA0; &#xA0; &#xA0; location &quot;*.php&quot; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fastcgi socket &quot;/run/php-fpm.sock&quot;<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; directory index index.php<br>&#xA0; &#xA0; &#xA0; root &quot;/htdocs&quot;<br>}<br><br>----<br>For date, timezone issues, copy /etc/localtime:<br>&#xA0; &#xA0; $cp /etc/localtime /var/www/etc/localtime<br><br>If &apos;localhost&apos; DNS name fails to resolve, copy /etc/hosts<br>&#xA0; &#xA0; $cp /etc/hosts /var/www/etc/hosts</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.openbsd.php)

**[⬆ to root](/)**