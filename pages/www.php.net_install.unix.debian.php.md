# Debian GNU/Linux installation notes





To refresh this document, perhaps it would be worth mentioning more modern methods to serve php content under apache httpd.

Specifically, the preferred method is now fastcgi, using either of those recipes:

(mod_fastcgi, httpd 2.2)
http://wiki.apache.org/httpd/php-fastcgi

(mod_fcgid, httpd 2.2)
http://wiki.apache.org/httpd/php-fcgid

(mod_proxy_fcgi, httpd 2.4)
http://wiki.apache.org/httpd/PHP-FPM

While the legacy mod_php approach is still applicable for some older installations, the fastcgi method is much faster, and require much less RAM to operate, based on similar traffic patterns.

Thank you!

  

#



Compiling PHP on Ubuntu boxes.

If you would like to compile PHP from source as opposed to relying on package maintainers, here&apos;s a list of packages, and commands you can run

STEP 1:
sudo apt-get install autoconf build-essential curl libtool \
&#xA0; libssl-dev libcurl4-openssl-dev libxml2-dev libreadline7 \
&#xA0; libreadline-dev libzip-dev libzip4 nginx openssl \
&#xA0; pkg-config zlib1g-dev

So you don&apos;t overwrite any existing PHP installs on your system, install PHP in your home directory. Create a directory for the PHP binaries to live

&#xA0; &#xA0; mkdir -p ~/bin/php7-latest/

STEP 2:
# download the latest PHP tarball, decompress it, then cd to the new directory.

STEP 3:
Configure PHP. Remove any options you don&apos;t need (like MySQL or Postgres (--with-pdo-pgsql))

./configure --prefix=$HOME/bin/php-latest \
&#xA0; &#xA0; --enable-mysqlnd \
&#xA0; &#xA0; --with-pdo-mysql \
&#xA0; &#xA0; --with-pdo-mysql=mysqlnd \
&#xA0; &#xA0; --with-pdo-pgsql=/usr/bin/pg_config \
&#xA0; &#xA0; --enable-bcmath \
&#xA0; &#xA0; --enable-fpm \
&#xA0; &#xA0; --with-fpm-user=www-data \
&#xA0; &#xA0; --with-fpm-group=www-data \
&#xA0; &#xA0; --enable-mbstring \
&#xA0; &#xA0; --enable-phpdbg \
&#xA0; &#xA0; --enable-shmop \
&#xA0; &#xA0; --enable-sockets \
&#xA0; &#xA0; --enable-sysvmsg \
&#xA0; &#xA0; --enable-sysvsem \
&#xA0; &#xA0; --enable-sysvshm \
&#xA0; &#xA0; --enable-zip \
&#xA0; &#xA0; --with-libzip=/usr/lib/x86_64-linux-gnu \
&#xA0; &#xA0; --with-zlib \
&#xA0; &#xA0; --with-curl \
&#xA0; &#xA0; --with-pear \
&#xA0; &#xA0; --with-openssl \
&#xA0; &#xA0; --enable-pcntl \
&#xA0; &#xA0; --with-readline

STEP 4:
compile the binaries by typing: make

If no errors, install by typing: make install

STEP 5:
Copy the PHP.ini file to the install directory

&#xA0; &#xA0; cp php.ini-development ~/bin/php-latest/lib/ 

STEP 6:

cd ~/bin/php-latest/etc; 
mv php-fpm.conf.default php-fpm.conf
mv php-fpm.d/www.conf.default php-fpm.d/www.conf

STEP 7:
create symbolic links for your for your binary files

&#xA0;&#xA0; cd ~/bin
&#xA0;&#xA0; ln -s php-latest/bin/php php
&#xA0;&#xA0; ln -s php-latest/bin/php-cgi php-cgi
&#xA0;&#xA0; ln -s php-latest/bin/php-config php-config
&#xA0;&#xA0; ln -s php-latest/bin/phpize phpize
&#xA0;&#xA0; ln -s php-latest/bin/phar.phar phar
&#xA0;&#xA0; ln -s php-latest/bin/pear pear
&#xA0;&#xA0; ln -s php-latest/bin/phpdbg phpdbg
&#xA0;&#xA0; ln -s php-latest/sbin/php-fpm php-fpm

STEP 8: link your local PHP to the php command. You will need to logout then log back in for php to switch to the local version instead of the installed version

# add this to .bashrc
if [ -d &quot;$HOME/bin&quot; ] ; then
&#xA0; PATH=&quot;$HOME/bin:$PATH&quot;
fi

STEP 9: Start PHP-FPM

&#xA0; &#xA0; sudo ~/bin/php7/sbin/php-fpm

  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.debian.php)

**[To root](/README.md)**