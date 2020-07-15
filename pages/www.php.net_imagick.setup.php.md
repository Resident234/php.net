# Installing/Configuring



Steps to Install ImageMagick on UwAmp for Windows:<br>as of March 31, 2016<br>Detailed guide for newbies like me.<br><br>... continued<br><br>4. Goto:<br><br>    http://pecl.php.net/package/imagick<br><br>    as of today, latest is 3.4.1 so I went to:<br>    http://pecl.php.net/package/imagick/3.4.1/windows<br><br>    My PHP version is: 5.6.18, and Thread Safety is Yes from<br>    Step #1, so I downloaded:<br>       5.6 Thread Safe (TS) x86<br>    I got: php_imagick-3.4.1-5.6-ts-vc11-x86.zip<br><br>5. Unzip and copy "php_imagick.dll" to the php extension folder:<br><br>    In my case:<br>    php_imagick.dll --&gt; C:\UwAmp\bin\php\?>
```
5.6.18\ext<br><br>    Note: this ZIP also contains dlls which other guides says<br>    to extract to the extension folder of apache.<br>    NO NEED TO DO IT. Step #3 has taken care of it.<br><br>6. Edit "php.ini" and add at the very end (could be <br>    anywhere I suppose):<br><br>      [Imagick]<br>      extension=php_imagick.dll<br><br>      For super newbies: click the edit button in the UwAmp UI,<br>      "php_uwamp.ini" will open and edit it. It will be copied to<br>      the correct php.ini when UwAmp is restarted. I had <br>      trouble at first since there are several php*.ini scattered <br>      all over.<br><br>7. Restart Apache<br><br>8. Check PHPInfo<br>    scroll to section (or find): imagick    <br>    number of supported formats: 234<br><br>    If there is no "imagick" section or "supported format" is 0,<br>    something went wrong.<br><br>Hope this helps.  

#

Steps to Install ImageMagick on UwAmp for Windows:<br>as of March 31, 2016<br>Detailed guide for newbies like me.<br>Took a long time to get it to work.<br><br>I initially followed:<br>http://php .net/manual/en/imagick.installation.php<br>but after installation, PHPInfo under imagick shows<br>number of supported formats = 0 <br><br>So I followed these steps, clobbered from various sources<br>to get it to work.<br><br>1. Open PHPInfo and check:<br>   Architecture = x86 or x64<br>   Thread Safety = yes or no<br><br>2. Download ImageMagick from:<br><br>   http://windows.php.net/downloads/pecl/deps/<br><br>   In my case I downloaded: ImageMagick-6.9.3-7-vc11-x86.zip<br>   because the Architecture under PHPInfo is x86<br>   as for vc11 or vc14<br>   search google for "visual c++ 11 runtime" or<br>   "visual c++ 14 runtime" and install it<br><br>3. Unzip and copy all dlls from the bin subfolder to the<br>    Apache bin directory. It&apos;s a bunch of CORE_RL_*.dll<br>    and IM_MOD_RL_*.dll plus a few other dlls.<br><br>    In my case, I installed UwAmp in C:\UwAmp, so:<br>    (from zip) bin/*.dll --&gt; C:\UwAmp\bin\apache\bin  

#

To install on Ubuntu or Debian, using the package manager, use:<br><br>sudo apt-get install php5-imagick<br>sudo service apache2 reload  

#

After 2 hours of looking for help from different documentation &amp; sites, I found out none of them are complete solution.  So, I summary my instruction here:<br><br>1) yum install ?>
```
devel<br>2) cd /usr<br>3) wget http://pear.php.net/go-pear<br>4) php go-pear<br>5) See the following line in /etc/php.ini [include_path=".:/usr/PEAR"]<br>6) pecl install imagick<br>7) Add the following line in /etc/php.ini [extension=imagick.so]<br>8) service httpd restart<br><br>Hopefully, I can save other engineer effort &amp; time.... Good luck!  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setup.php)

**[To root](/README.md)**