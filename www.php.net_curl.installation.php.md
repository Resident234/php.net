# Installation




<div class="phpcode"><span class="html">
Upgrading to php 5.6.9 on Windows 7 x64 curl no longer is recognised. No errors on server start package just not available and didn&apos;t show in phpinfo.php. deplister.exe was ok<br>I fixed coping the following list files from php folder (in my case D:\xampp\php)<br>libeay32.dll<br>libssh2.dll<br>ssleay32.dll<br>to c:\xampp\apache\bin (or your apache\bin path), restart apache and works fine, apache&apos;s libraries were outdated</span>
</div>
  

#


<div class="phpcode"><span class="html">
Ubuntu 11.04<br><br>I already had Apache and PHP5 setup, but simply adding php5-curl and curl did *not* work. I also had to get libcurl3 and libcurl3-dev. The full command:<br><br>sudo apt-get install curl libcurl3 libcurl3-dev php5-curl<br><br>You&apos;ll know if it works because phpinfo() will get a new section with Curl info.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is not necessary to always (re)compile PHP. For me it was sufficient to install php5-curl and restart Apache:<br><br>$ sudo apt-get install php5-curl<br>$ sudo /etc/init.d/apache2 restart</span>
</div>
  

#


<div class="phpcode"><span class="html">
You may be confused, as I was, by the instructions for installing cURL in php.&#xA0; The instruction &quot;To use PHP&apos;s cURL support you must also compile PHP --with-curl[=DIR]...&quot; was murky to me, since I didn&apos;t compile php when I installed it.&#xA0; I just copied all of the necessary files to the correct folders as described very clearly in the php manual.<br><br>I am using Windows XP and Apache with php 5.1.6. In this situation, and it may apply to php versions of 5.0 and later, all one needs to do is remove the &quot;;&quot; from the front of the directive extension=php_curl.dll.&#xA0; You should also check to make certain that libeay32.dll and ssleay32.dll are in your php directory with the other dll&apos;s.&#xA0; This directory should already be in you path, so the instruction to put them in you path is not critical.<br><br>You can then run phpinfo() and you should see a heading for curl in the listing.<br><br>Succinctly, my installation of cURL consisted of removing the semi-colon in front of the ;extension=php_curl.dll line in php.ini, saving php.ini and restarting Apache.&#xA0; You may wish to try this if you are using php 5.0 and later and are having difficulty understanding the instructions on the cURL installation page at php.net<br><br>You might also find the information at <a href="http://curl.phptrack.com/forum/viewtopic.php?t=52" rel="nofollow" target="_blank">http://curl.phptrack.com/forum/viewtopic.php?t=52</a> useful.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/curl.installation.php)

**[⬆ to root](/)**