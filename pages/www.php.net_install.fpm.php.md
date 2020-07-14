# FastCGI Process Manager (FPM)



Init script setup<br>===<br><br>You will probably want to create an init script for your new ?>
```
fpm. Fortunately, PHP 5.3.3 provides one for you, which you should copy to your init directory and change permissions:<br><br>$ cp &lt;?>
```
5.3.3-source-dir&gt;/sapi/fpm/init.d.?>
```
fpm.in /etc/init.d/?>
```
fpm<br>$ chmod 755 /etc/init.d/?>
```
fpm<br><br>It requires a certain amount of setup. First of all, make sure your ?>
```
fpm.conf file is set up to  create a PID file when ?>
```
fpm starts. E.g.:<br>----<br>pid = /var/run/?>
```
fpm.pid<br>----<br>(also make sure your ?>
```
fpm user has permission to create this file).<br><br>Now open up your new init script (/etc/init.d/?>
```
fpm) and set the variables at the top to their relevant values. E.g.:<br>---<br>prefix=<br>exec_prefix=<br>php_fpm_BIN=/sbin/?>
```
fpm<br>php_fpm_CONF=/etc/?>
```
fpm.conf<br>php_fpm_PID=/var/run/?>
```
fpm.pid<br>---<br><br>Your init script is now ready. You should now be able to start, stop and reload ?>
```
fpm:<br><br>$ /etc/init.d/?>
```
fpm start<br>$ /etc/init.d/?>
```
fpm stop<br>$ /etc/init.d/?>
```
fpm reload<br><br>The one remaining thing you may wish to do is to add your new ?>
```
fpm init script to system start-up. E.g. in CentOS:<br><br>$ /sbin/chkconfig ?>
```
fpm on<br><br>===========<br><br>Disclaimer: Although I did just do this on my own server about 20 mins ago, everything I&apos;ve written here is off the top of my head, so it may not be 100% correct. Also, allow for differences in system setup. Some understanding of what you are doing is assumed.  

#

PHP-FPM is FAST - but be wary of using it while your code base is stored on NFS - under average load your NFS server will feel some serious strain. I have yet to find a work around for this bug: https://bugs.php.net/bug.php?id=52312  

#

[Official documentation page](https://www.php.net/manual/en/install.fpm.php)

**[To root](/README.md)**