# Debian GNU/Linux installation notes




<div class="phpcode"><span class="html">
To refresh this document, perhaps it would be worth mentioning more modern methods to serve php content under apache httpd.<br><br>Specifically, the preferred method is now fastcgi, using either of those recipes:<br><br>(mod_fastcgi, httpd 2.2)<br><a href="http://wiki.apache.org/httpd/php-fastcgi" rel="nofollow" target="_blank">http://wiki.apache.org/httpd/php-fastcgi</a><br><br>(mod_fcgid, httpd 2.2)<br><a href="http://wiki.apache.org/httpd/php-fcgid" rel="nofollow" target="_blank">http://wiki.apache.org/httpd/php-fcgid</a><br><br>(mod_proxy_fcgi, httpd 2.4)<br><a href="http://wiki.apache.org/httpd/PHP-FPM" rel="nofollow" target="_blank">http://wiki.apache.org/httpd/PHP-FPM</a><br><br>While the legacy mod_php approach is still applicable for some older installations, the fastcgi method is much faster, and require much less RAM to operate, based on similar traffic patterns.<br><br>Thank you!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Compiling PHP on Ubuntu boxes.<br><br>If you would like to compile PHP from source as opposed to relying on package maintainers, here&apos;s a list of packages, and commands you can run<br><br>STEP 1:<br>sudo apt-get install autoconf build-essential curl libtool \<br>&#xA0; libssl-dev libcurl4-openssl-dev libxml2-dev libreadline7 \<br>&#xA0; libreadline-dev libzip-dev libzip4 nginx openssl \<br>&#xA0; pkg-config zlib1g-dev<br><br>So you don&apos;t overwrite any existing PHP installs on your system, install PHP in your home directory. Create a directory for the PHP binaries to live<br><br>&#xA0; &#xA0; mkdir -p ~/bin/php7-latest/<br><br>STEP 2:<br># download the latest PHP tarball, decompress it, then cd to the new directory.<br><br>STEP 3:<br>Configure PHP. Remove any options you don&apos;t need (like MySQL or Postgres (--with-pdo-pgsql))<br><br>./configure --prefix=$HOME/bin/php-latest \<br>&#xA0; &#xA0; --enable-mysqlnd \<br>&#xA0; &#xA0; --with-pdo-mysql \<br>&#xA0; &#xA0; --with-pdo-mysql=mysqlnd \<br>&#xA0; &#xA0; --with-pdo-pgsql=/usr/bin/pg_config \<br>&#xA0; &#xA0; --enable-bcmath \<br>&#xA0; &#xA0; --enable-fpm \<br>&#xA0; &#xA0; --with-fpm-user=www-data \<br>&#xA0; &#xA0; --with-fpm-group=www-data \<br>&#xA0; &#xA0; --enable-mbstring \<br>&#xA0; &#xA0; --enable-phpdbg \<br>&#xA0; &#xA0; --enable-shmop \<br>&#xA0; &#xA0; --enable-sockets \<br>&#xA0; &#xA0; --enable-sysvmsg \<br>&#xA0; &#xA0; --enable-sysvsem \<br>&#xA0; &#xA0; --enable-sysvshm \<br>&#xA0; &#xA0; --enable-zip \<br>&#xA0; &#xA0; --with-libzip=/usr/lib/x86_64-linux-gnu \<br>&#xA0; &#xA0; --with-zlib \<br>&#xA0; &#xA0; --with-curl \<br>&#xA0; &#xA0; --with-pear \<br>&#xA0; &#xA0; --with-openssl \<br>&#xA0; &#xA0; --enable-pcntl \<br>&#xA0; &#xA0; --with-readline<br><br>STEP 4:<br>compile the binaries by typing: make<br><br>If no errors, install by typing: make install<br><br>STEP 5:<br>Copy the PHP.ini file to the install directory<br><br>&#xA0; &#xA0; cp php.ini-development ~/bin/php-latest/lib/ <br><br>STEP 6:<br><br>cd ~/bin/php-latest/etc; <br>mv php-fpm.conf.default php-fpm.conf<br>mv php-fpm.d/www.conf.default php-fpm.d/www.conf<br><br>STEP 7:<br>create symbolic links for your for your binary files<br><br>&#xA0;&#xA0; cd ~/bin<br>&#xA0;&#xA0; ln -s php-latest/bin/php php<br>&#xA0;&#xA0; ln -s php-latest/bin/php-cgi php-cgi<br>&#xA0;&#xA0; ln -s php-latest/bin/php-config php-config<br>&#xA0;&#xA0; ln -s php-latest/bin/phpize phpize<br>&#xA0;&#xA0; ln -s php-latest/bin/phar.phar phar<br>&#xA0;&#xA0; ln -s php-latest/bin/pear pear<br>&#xA0;&#xA0; ln -s php-latest/bin/phpdbg phpdbg<br>&#xA0;&#xA0; ln -s php-latest/sbin/php-fpm php-fpm<br><br>STEP 8: link your local PHP to the php command. You will need to logout then log back in for php to switch to the local version instead of the installed version<br><br># add this to .bashrc<br>if [ -d &quot;$HOME/bin&quot; ] ; then<br>&#xA0; PATH=&quot;$HOME/bin:$PATH&quot;<br>fi<br><br>STEP 9: Start PHP-FPM<br><br>&#xA0; &#xA0; sudo ~/bin/php7/sbin/php-fpm</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.debian.php)

**[To root](/README.md)**