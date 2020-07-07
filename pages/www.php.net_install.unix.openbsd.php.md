# OpenBSD installation notes





A brief update: As of OpenBSD 5.7 (2015) the installation process is extremely easy. Apache httpd was replaced by Nginx, which has since been further replaced by OpenBSD&apos;s own server, aptly named &apos;httpd&apos;. 

&apos;httpd&apos; is installed by default, everything else you can still get from packages, with a couple name changes (including Apache and Nginx.) You will be asked which version to install - at the time of writing, versions 5.3.29p1 thru 5.6.5 are available.

#pkg_add php
#pkg_add php-fpm
#pkg_add pear

----
OpenBSD disables most services by default; a blank &apos;_flags&apos; line overrides default &apos;NO&apos; value. pkg_scripts are located in /etc/rc.d/
To start at boot, edit &quot;/etc/rc.conf.local&quot;:

&#xA0; httpd_flags=
&#xA0; pkg_scripts=php_fpm

----
Example /etc/httpd.conf
#
# paths are relative to chroot - e.g, &apos;/var/www/run/php-fpm.sock&apos;
server &quot;default&quot; {
&#xA0; &#xA0; &#xA0; listen on * port 80
&#xA0; &#xA0; &#xA0; location &quot;*.php&quot; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fastcgi socket &quot;/run/php-fpm.sock&quot;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; directory index index.php
&#xA0; &#xA0; &#xA0; root &quot;/htdocs&quot;
}

----
For date, timezone issues, copy /etc/localtime:
&#xA0; &#xA0; $cp /etc/localtime /var/www/etc/localtime

If &apos;localhost&apos; DNS name fails to resolve, copy /etc/hosts
&#xA0; &#xA0; $cp /etc/hosts /var/www/etc/hosts

  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.openbsd.php)

**[To root](/README.md)**