# FastCGI Process Manager (FPM)



Init script setup<br>===<br><br>You will probably want to create an init script for your new php-fpm. Fortunately, PHP 5.3.3 provides one for you, which you should copy to your init directory and change permissions:<br><br>$ cp 

```
<?php-5.3.3-source-dir>/sapi/fpm/init.d.php-fpm.in /etc/init.d/php-fpm
$ chmod 755 /etc/init.d/php-fpm

It requires a certain amount of setup. First of all, make sure your php-fpm.conf file is set up to  create a PID file when php-fpm starts. E.g.:
----
pid = /var/run/php-fpm.pid
----
(also make sure your php-fpm user has permission to create this file).

Now open up your new init script (/etc/init.d/php-fpm) and set the variables at the top to their relevant values. E.g.:
---
prefix=
exec_prefix=
php_fpm_BIN=/sbin/php-fpm
php_fpm_CONF=/etc/php-fpm.conf
php_fpm_PID=/var/run/php-fpm.pid
---

Your init script is now ready. You should now be able to start, stop and reload php-fpm:

$ /etc/init.d/php-fpm start
$ /etc/init.d/php-fpm stop
$ /etc/init.d/php-fpm reload

The one remaining thing you may wish to do is to add your new php-fpm init script to system start-up. E.g. in CentOS:

$ /sbin/chkconfig php-fpm on

===========

Disclaimer: Although I did just do this on my own server about 20 mins ago, everything I've written here is off the top of my head, so it may not be 100% correct. Also, allow for differences in system setup. Some understanding of what you are doing is assumed.?>
```
  

---

PHP-FPM is FAST - but be wary of using it while your code base is stored on NFS - under average load your NFS server will feel some serious strain. I have yet to find a work around for this bug: https://bugs.php.net/bug.php?id=52312  

---

[Official documentation page](https://www.php.net/manual/en/install.fpm.php)

**[To root](/README.md)**