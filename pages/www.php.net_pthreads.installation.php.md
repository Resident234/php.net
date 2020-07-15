# Installation



Here is how I got it working under Linux Ubuntu distro - WITHOUT USE PECL:<br><br>We will download both, PHP and Pthread without PECL<br><br>1 - Get PHP version<br>For this example we will use version: 5.4.36<br><br># wget http://www.php.net/distributions/?>
```
5.4.36.tar.gz<br><br>2- Get Pthreads version:<br>I&apos;m using an old version but, you could take any one<br><br># wget http://pecl.php.net/get/pthreads-1.0.0.tgz<br><br>Extract both, php and pthreads versions<br><br>#tar zxvf ?>
```
5.4.36.tar.gz<br>#tar zxvf pthreads-1.0.0.tgz <br><br>3- Move Pthreads to php/ext folder. Inside version of PHP downloaded at item 1.<br><br>4- Reconfigure sources<br># ./buildconf --force<br># ./configure --help | grep pthreads<br><br>You have to see --enable-pthreads listed. If do not, clear the buidls with this commands:<br><br># rm -rf aclocal.m4<br># rm -rf autom4te.cache/<br># ./buildconf --force<br><br>5 - Inside php folder run configure command to set what we need:<br># ./configure --enable-debug --enable-maintainer-zts --enable-pthreads --prefix=/usr --with-config-file-path=/etc<br><br>6 - Install PHP<br>We will run make clear just to be sure that no other crashed build will mess our new one.<br><br># make clear <br># make<br># make install<br><br>7 - Copy configuration file of PHP and add local lib to include path<br># cp php.ini-development /etc/php.ini<br><br>Edit php.ini and set Include_path to be like this:<br><br>Include_path = &#x201C;/usr/local/lib/php&#x201D;<br><br>9 - Check Modules<br># php -m (check pthread loaded)<br><br>You have to see pthreads listed<br><br>10 - If pthread is not listed, update php.ini<br># echo "extension=pthreads.so" &gt;&gt; /etc/php.ini  

#

For Wampp (Windows)<br>-----------------------------------------------------------------------------------<br>1.  Find out what is your &apos;PHP Extension Build&apos; version by using phpinfo(). You can use this - http://localhost/?phpinfo=1<br><br>2.  Download the pthreads that matches your php version (32 bit or 64 bit) and php extension build (currently used VC11). Use this link for download - http://windows.php.net/downloads/pecl/releases/pthreads/ <br><br>3.  Extract the zip -<br>      Move php_pthreads.dll to the &apos;bin\php\ext\&apos; directory.<br>      Move pthreadVC2.dll to the &apos;bin\php\&apos; directory.<br>      Move pthreadVC2.dll to the &apos;bin\apache\bin&apos; directory.<br>      Move pthreadVC2.dll to the &apos;C:\windows\system32&apos; directory.<br><br>4.  Open php\php.ini and add<br>      extension=php_pthreads.dll<br><br>Now restart server and you are done. Thanks.  

#

On Windows the installation is as follows:<br><br>Download the pthreads that matches your php version.<br>I found mine at: http://windows.php.net/downloads/pecl/releases/pthreads/<br>(I used version 0.44 wich is the newest at the time of writing this, and then downloaded the one for php 5.3 which is the version I am using).<br><br>Extract the zip.<br>Move php_pthreads.dll to the php\ext\ directory.<br>Move pthreadVC2.dll to the php\ directory.<br><br>Open php\php.ini and add<br>extension=php_pthreads.dll<br><br>You are done.  

#

I haven&apos;t found any proper instructions on how to install pthreads in linux, so I&apos;ll leave the steps I followed:<br><br># Required libraries<br>sudo apt-get install gcc make libzzip-dev libreadline-dev libxml2-dev \<br>libssl-dev libmcrypt-dev libcurl4-openssl-dev lib32bz2-dev <br><br># Download PHP<br>cd /usr/local/src<br><br>wget http://www.php.net/distributions/?>
```
&lt;version&gt;.tar.gz<br>( e.g. wget http://www.php.net/distributions/?>
```
5.5.8.tar.gz )<br><br># Extract<br>tar zxvf ?>
```
&lt;version&gt;.tar.gz<br>(e.g. tar zxvf ?>
```
5.5.8.tar.gz )<br><br># Configure<br>cd /usr/local/src/?>
```
&lt;version&gt;<br>( e.g. cd /usr/local/src/?>
```
5.5.8 )<br><br>./configure --prefix=/usr --with-config-file-path=/etc --enable-maintainer-zts<br><br># Compile<br>make &amp;&amp; make install<br>( make -j3 &amp;&amp; make -j3 install) -&gt; Faster building<br><br># Copy configuration<br>cp php.ini-development /etc/php.ini<br><br># Install pthreads<br>pecl install pthreads<br>echo "extension=pthreads.so" &gt;&gt; /etc/php.ini<br><br># Check installation<br>php -m | grep pthreads  

#

[Official documentation page](https://www.php.net/manual/en/pthreads.installation.php)

**[To root](/README.md)**