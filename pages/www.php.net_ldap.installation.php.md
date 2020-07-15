# Installation



I found not only "Versions before PHP 4.3.0 additionally require libsasl.dll.".<br><br>If you use ?>
```
5.3.3-Win32-VC9-x86 or later Versions that<br>It&apos;s require libsasl.dll.<br><br>Running under Windows &amp; Apache 2.2.8<br>PHP file is download from http://windows.php.net/downloads/releases/archives/<br><br>When I use ?>
```
5.2.x-Win32-VC6-x86 and ?>
```
5.3.x-Win32-VC6-x86<br><br>1.just uncomment extension=php_ldap.dll  in php.ini<br>2.Restart apache,it&apos;s ok<br><br>When I use ?>
```
5.3.x-Win32-VC9-x86 and ?>
```
5.4.x-Win32-VC9-x86<br><br>1.just uncomment extension=php_ldap.dll  in php.ini<br>2.Restart apache,always fail...<br>(only ?>
```
5.3.1-Win32-VC9-x86 &amp; ?>
```
5.3.2-Win32-VC9-x86 is ok. )<br><br>[?>
```
5.3.3-Win32-VC9-x86 or later Versions]<br>1.just uncomment extension=php_ldap.dll  in php.ini<br>2.copy  libsasl.dll to [apache folder]\bin<br>3.Restart apache,it&apos;s ok  

#

If using a debian machine (debian or ubuntu variants) just do apt-get install php5-ldap, that&apos;s all to get ldap work on php. No need to get sources, try to compiling them and  so on.  

#

[Official documentation page](https://www.php.net/manual/en/ldap.installation.php)

**[To root](/README.md)**