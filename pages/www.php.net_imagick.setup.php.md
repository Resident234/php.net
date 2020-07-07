# Installing/Configuring





Steps to Install ImageMagick on UwAmp for Windows:
as of March 31, 2016
Detailed guide for newbies like me.

... continued

4. Goto:

&#xA0; &#xA0; http://pecl.php.net/package/imagick

&#xA0; &#xA0; as of today, latest is 3.4.1 so I went to:
&#xA0; &#xA0; http://pecl.php.net/package/imagick/3.4.1/windows

&#xA0; &#xA0; My PHP version is: 5.6.18, and Thread Safety is Yes from
&#xA0; &#xA0; Step #1, so I downloaded:
&#xA0; &#xA0; &#xA0;&#xA0; 5.6 Thread Safe (TS) x86
&#xA0; &#xA0; I got: php_imagick-3.4.1-5.6-ts-vc11-x86.zip

5. Unzip and copy &quot;php_imagick.dll&quot; to the php extension folder:

&#xA0; &#xA0; In my case:
&#xA0; &#xA0; php_imagick.dll --&gt; C:\UwAmp\bin\php\php-5.6.18\ext

&#xA0; &#xA0; Note: this ZIP also contains dlls which other guides says
&#xA0; &#xA0; to extract to the extension folder of apache.
&#xA0; &#xA0; NO NEED TO DO IT. Step #3 has taken care of it.

6. Edit &quot;php.ini&quot; and add at the very end (could be 
&#xA0; &#xA0; anywhere I suppose):

&#xA0; &#xA0; &#xA0; [Imagick]
&#xA0; &#xA0; &#xA0; extension=php_imagick.dll

&#xA0; &#xA0; &#xA0; For super newbies: click the edit button in the UwAmp UI,
&#xA0; &#xA0; &#xA0; &quot;php_uwamp.ini&quot; will open and edit it. It will be copied to
&#xA0; &#xA0; &#xA0; the correct php.ini when UwAmp is restarted. I had 
&#xA0; &#xA0; &#xA0; trouble at first since there are several php*.ini scattered 
&#xA0; &#xA0; &#xA0; all over.

7. Restart Apache

8. Check PHPInfo
&#xA0; &#xA0; scroll to section (or find): imagick&#xA0; &#xA0; 
&#xA0; &#xA0; number of supported formats: 234

&#xA0; &#xA0; If there is no &quot;imagick&quot; section or &quot;supported format&quot; is 0,
&#xA0; &#xA0; something went wrong.

Hope this helps.

  

#



Steps to Install ImageMagick on UwAmp for Windows:
as of March 31, 2016
Detailed guide for newbies like me.
Took a long time to get it to work.

I initially followed:
http://php .net/manual/en/imagick.installation.php
but after installation, PHPInfo under imagick shows
number of supported formats = 0 

So I followed these steps, clobbered from various sources
to get it to work.

1. Open PHPInfo and check:
&#xA0;&#xA0; Architecture = x86 or x64
&#xA0;&#xA0; Thread Safety = yes or no

2. Download ImageMagick from:

&#xA0;&#xA0; http://windows.php.net/downloads/pecl/deps/

&#xA0;&#xA0; In my case I downloaded: ImageMagick-6.9.3-7-vc11-x86.zip
&#xA0;&#xA0; because the Architecture under PHPInfo is x86
&#xA0;&#xA0; as for vc11 or vc14
&#xA0;&#xA0; search google for &quot;visual c++ 11 runtime&quot; or
&#xA0;&#xA0; &quot;visual c++ 14 runtime&quot; and install it

3. Unzip and copy all dlls from the bin subfolder to the
&#xA0; &#xA0; Apache bin directory. It&apos;s a bunch of CORE_RL_*.dll
&#xA0; &#xA0; and IM_MOD_RL_*.dll plus a few other dlls.

&#xA0; &#xA0; In my case, I installed UwAmp in C:\UwAmp, so:
&#xA0; &#xA0; (from zip) bin/*.dll --&gt; C:\UwAmp\bin\apache\bin

  

#



To install on Ubuntu or Debian, using the package manager, use:

sudo apt-get install php5-imagick
sudo service apache2 reload

  

#



After 2 hours of looking for help from different documentation &amp; sites, I found out none of them are complete solution.&#xA0; So, I summary my instruction here:

1) yum install php-devel
2) cd /usr
3) wget http://pear.php.net/go-pear
4) php go-pear
5) See the following line in /etc/php.ini [include_path=&quot;.:/usr/PEAR&quot;]
6) pecl install imagick
7) Add the following line in /etc/php.ini [extension=imagick.so]
8) service httpd restart

Hopefully, I can save other engineer effort &amp; time.... Good luck!

  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setup.php)

**[To root](/README.md)**