# Installing/Configuring





http://www.apachefriends.org/en/xampp-macosx.html

This links to xampp works great on macs. I have put it only for PPC machines, but it claims to work properly on Intel as well. 

I also had success installing FreeTDS and PHP from source on an Intel Macbook, running Mac OS 10.6 (Snow Leopard). My steps were the following:

PREPARE SOURCE CODE
step 1: download latest php version
step 2: un-tar the php source code
step 3: download latest freetds version
step 4: un-tar the freetds source code

INSTALL THE FREETDS
step 5: make sure there is a writable directory at /usr/local/freetds
step 6: &quot;cd&quot; to the freetds source directory
step 7: run &quot;sudo ./configure --prefix=/usr/local/freetds --enable-msdblib&quot;
step 8: run &quot;sudo make&quot;
step 9: run &quot;sudo make install&quot;
step 10: run &quot;touch /usr/local/freetds/include/tds.h&quot; (add blank, but necessary files)
step 11: run &quot;touch /usr/local/freetds/lib/libtds.a&quot; (add blank, but necessary files)

INSTALL THE PHP
step 11: &quot;cd&quot; to the php source directory
step 12: run &quot;sudo ./configure --disable-all --with-mssql=/usr/local/freetds&quot;
step 13: run &quot;sudo make&quot;
step 14: run &quot;sudo make install&quot;

Of course, since I was disabling-all (step 12) that means that you have to explicitly add back those modules you want to configure php with.

  

#

[Official documentation page](https://www.php.net/manual/en/mssql.setup.php)

**[To root](/README.md)**