# Installation




<div class="phpcode"><span class="html">
I found not only &quot;Versions before PHP 4.3.0 additionally require libsasl.dll.&quot;.<br><br>If you use php-5.3.3-Win32-VC9-x86 or later Versions that<br>It&apos;s require libsasl.dll.<br><br>Running under Windows &amp; Apache 2.2.8<br>PHP file is download from <a href="http://windows.php.net/downloads/releases/archives/" rel="nofollow" target="_blank">http://windows.php.net/downloads/releases/archives/</a><br><br>When I use php-5.2.x-Win32-VC6-x86 and php-5.3.x-Win32-VC6-x86<br><br>1.just uncomment extension=php_ldap.dll&#xA0; in php.ini<br>2.Restart apache,it&apos;s ok<br><br>When I use php-5.3.x-Win32-VC9-x86 and php-5.4.x-Win32-VC9-x86<br><br>1.just uncomment extension=php_ldap.dll&#xA0; in php.ini<br>2.Restart apache,always fail...<br>(only php-5.3.1-Win32-VC9-x86 &amp; php-5.3.2-Win32-VC9-x86 is ok. )<br><br>[php-5.3.3-Win32-VC9-x86 or later Versions]<br>1.just uncomment extension=php_ldap.dll&#xA0; in php.ini<br>2.copy&#xA0; libsasl.dll to [apache folder]\bin<br>3.Restart apache,it&apos;s ok</span>
</div>
  

#


<div class="phpcode"><span class="html">
If using a debian machine (debian or ubuntu variants) just do apt-get install php5-ldap, that&apos;s all to get ldap work on php. No need to get sources, try to compiling them and&#xA0; so on.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ldap.installation.php)

**[⬆ to root](/)**