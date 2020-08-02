# Installation



CentOS 6.2 64bit Installation Steps:<br><br>1. download the libssh2 package from http://libssh2.org, command as following: <br>tar vxzf libssh2-1.4.2.tar.gz<br>cd libssh2-1.4.2<br>./configure<br>make<br>make install<br><br>2. download the php-ssh2 package from http://pecl.php.net/package/ssh2:<br><br>tar vxzf ssh2-0.11.3<br>cd ssh2-0.11.3<br>phpize<br>./configure --with-ssh2<br>make<br>make install<br><br>and the ssh2.so file will copy into /usr/lib64/php/modules<br>check it.<br><br>3. modify the php.ini<br><br>vi /etc/php.ini<br><br>add the "extension=ssh2.so" to the extension part of php.ini<br><br>4. check the environment of php, use phpinfo();<br><br>5. enjoy  

---

On Ubuntu 16.04 LTS<br>- PHP 5.6.24-1+deb.sury.org~xenial+1<br><br>Using terminal as a root<br>     apt install php-ssh2<br>     service apache2 restart  

---

Steps for installing the extension package on Debian systems:<br><br>&gt; sudo apt-get install libssh2-php<br>&gt; sudo /etc/init.d/apache2 restart  

---

[Official documentation page](https://www.php.net/manual/en/ssh2.installation.php)

**[To root](/README.md)**