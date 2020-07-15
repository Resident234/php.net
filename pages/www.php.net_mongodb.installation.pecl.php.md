# Installing the MongoDB PHP Driver with PECL



An additional requirement might be pkg-config (on Ubuntu 14.04).<br><br>$ pecl install mongodb<br>...<br>configure: error: Cannot find OpenSSL&apos;s libraries<br>ERROR: `/tmp/pear/temp/mongodb/configure --with

```
<??>
```
config=/usr/bin/?>
```
config&apos; failed<br><br>But:<br><br>$ apt-get install pkg-config<br>...<br>Setting up pkg-config (0.26-1ubuntu4) ...<br>$ pecl install mongodb<br>...<br>Build process completed successfully<br>Installing &apos;/usr/lib/php/20151012/mongodb.so&apos;<br>install ok: channel://pecl.php.net/mongodb-1.1.7<br>configuration option "php_ini" is not set to php.ini location<br>You should add "extension=mongodb.so" to php.ini  

#

Ubuntu 16.04<br><br>sudo apt-get install libcurl4-openssl-dev pkg-config libssl-dev libsslcommon2-dev<br><br>sudo pecl install mongodb<br><br>add extension=mongodb.so in fpm and cli:<br><br>sudo vim /etc/php/7.0/fpm/conf.d/30-mongodb.ini<br><br>sudo vim /etc/php/7.0/cli/conf.d/30-mongodb.ini<br><br>Restart service:<br><br>sudo systemctl restart php7.0-fpm<br><br>sudo systemctl reload nginx  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.installation.pecl.php)

**[To root](/README.md)**