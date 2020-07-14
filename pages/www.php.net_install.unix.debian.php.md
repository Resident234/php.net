# Debian GNU/Linux installation notes



To refresh this document, perhaps it would be worth mentioning more modern methods to serve php content under apache httpd.<br><br>Specifically, the preferred method is now fastcgi, using either of those recipes:<br><br>(mod_fastcgi, httpd 2.2)<br>http://wiki.apache.org/httpd/?>
```
fastcgi<br><br>(mod_fcgid, httpd 2.2)<br>http://wiki.apache.org/httpd/?>
```
fcgid<br><br>(mod_proxy_fcgi, httpd 2.4)<br>http://wiki.apache.org/httpd/PHP-FPM<br><br>While the legacy mod_php approach is still applicable for some older installations, the fastcgi method is much faster, and require much less RAM to operate, based on similar traffic patterns.<br><br>Thank you!  

#

Compiling PHP on Ubuntu boxes.<br><br>If you would like to compile PHP from source as opposed to relying on package maintainers, here&apos;s a list of packages, and commands you can run<br><br>STEP 1:<br>sudo apt-get install autoconf build-essential curl libtool \<br>  libssl-dev libcurl4-openssl-dev libxml2-dev libreadline7 \<br>  libreadline-dev libzip-dev libzip4 nginx openssl \<br>  pkg-config zlib1g-dev<br><br>So you don&apos;t overwrite any existing PHP installs on your system, install PHP in your home directory. Create a directory for the PHP binaries to live<br><br>    mkdir -p ~/bin/php7-latest/<br><br>STEP 2:<br># download the latest PHP tarball, decompress it, then cd to the new directory.<br><br>STEP 3:<br>Configure PHP. Remove any options you don&apos;t need (like MySQL or Postgres (--with-pdo-pgsql))<br><br>./configure --prefix=$HOME/bin/?>
```
latest \<br>    --enable-mysqlnd \<br>    --with-pdo-mysql \<br>    --with-pdo-mysql=mysqlnd \<br>    --with-pdo-pgsql=/usr/bin/pg_config \<br>    --enable-bcmath \<br>    --enable-fpm \<br>    --with-fpm-user=www-data \<br>    --with-fpm-group=www-data \<br>    --enable-mbstring \<br>    --enable

```
<?phpdbg \
    --enable-shmop \
    --enable-sockets \
    --enable-sysvmsg \
    --enable-sysvsem \
    --enable-sysvshm \
    --enable-zip \
    --with-libzip=/usr/lib/x86_64-linux-gnu \
    --with-zlib \
    --with-curl \
    --with-pear \
    --with-openssl \
    --enable-pcntl \
    --with-readline

STEP 4:
compile the binaries by typing: make

If no errors, install by typing: make install

STEP 5:
Copy the PHP.ini file to the install directory

    cp php.ini-development ~/bin/?>
```
latest/lib/ 

STEP 6:

cd ~/bin/?>
```
latest/etc; 
mv ?>
```
fpm.conf.default ?>
```
fpm.conf
mv ?>
```
fpm.d/www.conf.default ?>
```
fpm.d/www.conf

STEP 7:
create symbolic links for your for your binary files

   cd ~/bin
   ln -s ?>
```
latest/bin/php php
   ln -s ?>
```
latest/bin/?>
```
cgi ?>
```
cgi
   ln -s ?>
```
latest/bin/?>
```
config ?>
```
config
   ln -s ?>
```
latest/bin/phpize phpize
   ln -s ?>
```
latest/bin/phar.phar phar
   ln -s ?>
```
latest/bin/pear pear
   ln -s ?>
```
latest/bin/phpdbg phpdbg
   ln -s ?>
```
latest/sbin/?>
```
fpm ?>
```
fpm

STEP 8: link your local PHP to the php command. You will need to logout then log back in for php to switch to the local version instead of the installed version

# add this to .bashrc
if [ -d "$HOME/bin" ] ; then
  PATH="$HOME/bin:$PATH"
fi

STEP 9: Start PHP-FPM

    sudo ~/bin/php7/sbin/?>
```
fpm  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.debian.php)

**[To root](/README.md)**