# Installing/Configuring



http://www.apachefriends.org/en/xampp-macosx.html<br><br>This links to xampp works great on macs. I have put it only for PPC machines, but it claims to work properly on Intel as well. <br><br>I also had success installing FreeTDS and PHP from source on an Intel Macbook, running Mac OS 10.6 (Snow Leopard). My steps were the following:<br><br>PREPARE SOURCE CODE<br>step 1: download latest php version<br>step 2: un-tar the php source code<br>step 3: download latest freetds version<br>step 4: un-tar the freetds source code<br><br>INSTALL THE FREETDS<br>step 5: make sure there is a writable directory at /usr/local/freetds<br>step 6: "cd" to the freetds source directory<br>step 7: run "sudo ./configure --prefix=/usr/local/freetds --enable-msdblib"<br>step 8: run "sudo make"<br>step 9: run "sudo make install"<br>step 10: run "touch /usr/local/freetds/include/tds.h" (add blank, but necessary files)<br>step 11: run "touch /usr/local/freetds/lib/libtds.a" (add blank, but necessary files)<br><br>INSTALL THE PHP<br>step 11: "cd" to the php source directory<br>step 12: run "sudo ./configure --disable-all --with-mssql=/usr/local/freetds"<br>step 13: run "sudo make"<br>step 14: run "sudo make install"<br><br>Of course, since I was disabling-all (step 12) that means that you have to explicitly add back those modules you want to configure php with.  

#

[Official documentation page](https://www.php.net/manual/en/mssql.setup.php)

**[To root](/README.md)**