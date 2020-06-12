# Installing/Configuring




<div class="phpcode"><span class="html">
<a href="http://www.apachefriends.org/en/xampp-macosx.html" rel="nofollow" target="_blank">http://www.apachefriends.org/en/xampp-macosx.html</a><br><br>This links to xampp works great on macs. I have put it only for PPC machines, but it claims to work properly on Intel as well. <br><br>I also had success installing FreeTDS and PHP from source on an Intel Macbook, running Mac OS 10.6 (Snow Leopard). My steps were the following:<br><br>PREPARE SOURCE CODE<br>step 1: download latest php version<br>step 2: un-tar the php source code<br>step 3: download latest freetds version<br>step 4: un-tar the freetds source code<br><br>INSTALL THE FREETDS<br>step 5: make sure there is a writable directory at /usr/local/freetds<br>step 6: &quot;cd&quot; to the freetds source directory<br>step 7: run &quot;sudo ./configure --prefix=/usr/local/freetds --enable-msdblib&quot;<br>step 8: run &quot;sudo make&quot;<br>step 9: run &quot;sudo make install&quot;<br>step 10: run &quot;touch /usr/local/freetds/include/tds.h&quot; (add blank, but necessary files)<br>step 11: run &quot;touch /usr/local/freetds/lib/libtds.a&quot; (add blank, but necessary files)<br><br>INSTALL THE PHP<br>step 11: &quot;cd&quot; to the php source directory<br>step 12: run &quot;sudo ./configure --disable-all --with-mssql=/usr/local/freetds&quot;<br>step 13: run &quot;sudo make&quot;<br>step 14: run &quot;sudo make install&quot;<br><br>Of course, since I was disabling-all (step 12) that means that you have to explicitly add back those modules you want to configure php with.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mssql.setup.php)

**[⬆ to root](/)**