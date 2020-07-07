# Installing the MongoDB PHP Driver with PECL





An additional requirement might be pkg-config (on Ubuntu 14.04).

$ pecl install mongodb
...
configure: error: Cannot find OpenSSL&apos;s libraries
ERROR: `/tmp/pear/temp/mongodb/configure --with-php-config=/usr/bin/php-config&apos; failed

But:

$ apt-get install pkg-config
...
Setting up pkg-config (0.26-1ubuntu4) ...
$ pecl install mongodb
...
Build process completed successfully
Installing &apos;/usr/lib/php/20151012/mongodb.so&apos;
install ok: channel://pecl.php.net/mongodb-1.1.7
configuration option &quot;php_ini&quot; is not set to php.ini location
You should add &quot;extension=mongodb.so&quot; to php.ini

  

#



Ubuntu 16.04

sudo apt-get install libcurl4-openssl-dev pkg-config libssl-dev libsslcommon2-dev

sudo pecl install mongodb

add extension=mongodb.so in fpm and cli:

sudo vim /etc/php/7.0/fpm/conf.d/30-mongodb.ini

sudo vim /etc/php/7.0/cli/conf.d/30-mongodb.ini

Restart service:

sudo systemctl restart php7.0-fpm

sudo systemctl reload nginx

  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.installation.pecl.php)

**[To root](/README.md)**