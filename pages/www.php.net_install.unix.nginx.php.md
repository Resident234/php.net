# Nginx 1.4.x on Unix systems





Building from source is not easy if something is a bit different, and I had a hard time with some directory and configuration options. I was floundering around the web until I found this site that translated from Chinese. No one else had the solution.&#xA0; I couldn&apos;t get php fpm to start until I changed the directory (Item 2.ERROR: Unable to globalize). I had other issues listed but I was able to solve them. Please don&apos;t delete this, it is very useful info.

The original site&#xA0; (it is in Chinese, not my site, but I want to give credit):

(there is some more there, you can goto the site)

blog.dream1987.top/?paged=2

Installation problems:

1. configure: error:. Xml2-config not found Please check your libxml2 installation.

solution:

apt-get install libxml2-dev

 

2.Warning: Declaration of PEAR_Installer :: download () should be compatible with &amp; PEAR_Downloader :: download ($ params) in phar: ///root/php-7.0.0alpha1/pear/install-pear-nozlib.phar/PEAR /Installer.php on line 43

Warning: Declaration of PEAR_PackageFile_Parser_v2 :: parse () should be compatible with PEAR_XMLParser :: parse ($ data) in phar: ///root/php-7.0.0alpha1/pear/install-pear-nozlib.phar/PEAR/PackageFile/ Parser / v2.php on line 113 
[PEAR] Archive_Tar - already installed: 1.3.13 
[PEAR] Console_Getopt - already installed: 1.3.1 
[PEAR] Structures_Graph- already installed: 1.0.4

Warning: Declaration of PEAR_Task_Replace :: init () should be compatible with PEAR_Task_Common :: init ($ xml, $ fileAttributes, $ lastVersion) in phar: ///root/php-7.0.0alpha1/pear/install-pear-nozlib. phar / PEAR / Task / Replace.php on line 31 
[PEAR] XML_Util - already installed: 1.2.3

Warning: Declaration of PEAR_Task_Windowseol :: init () should be compatible with PEAR_Task_Common :: init ($ xml, $ fileAttributes, $ lastVersion) in phar: ///root/php-7.0.0alpha1/pear/install-pear-nozlib. phar / PEAR / Task / Windowseol.php on line 76

Warning: Declaration of PEAR_Task_Unixeol :: init () should be compatible with PEAR_Task_Common :: init ($ xml, $ fileAttributes, $ lastVersion) in phar: ///root/php-7.0.0alpha1/pear/install-pear-nozlib. phar / PEAR / Task / Unixeol.php on line 76 
[PEAR] PEAR - already installed: 1.9.5

solution:

Workaround not found (http://pear.php.net/bugs/bug.php?id=20554)

3. Start php-fpm

1.ERROR: failed to open configuration file &apos;/usr/local/etc/php-fpm.conf&apos;: No such file or directory (2) 
ERROR: failed to load configuration file &apos;/usr/local/etc/php-fpm.conf&apos; 
ERROR: FPM initialization failed

solution:

Php-fpm.conf copy files from the source file to that location.

cp /root/php-7.0.0alpha1/sapi/fpm/php-fpm.conf /usr/local/etc/php-fpm.conf

2.ERROR: Unable to globalize &apos;/usr/local/NONE/etc/php-fpm.d/*.conf&apos; (ret = 2) from /usr/local/etc/php-fpm.conf at line 125. 
ERROR: failed to load configuration file &apos;/usr/local/etc/php-fpm.conf&apos; 
ERROR: FPM initialization failed

solution:

Edit /usr/local/etc/php-fpm.conf document introduced * .conf part, change to the correct path include = / usr / local / etc / php-fpm.d / *. Conf

If there is no /usr/local/etc/php-fpm.d directory, create the directory.

3.WARNING: Nothing matches the include pattern &apos;/usr/local/etc/php-fpm.d/*.conf&apos; from /usr/local/etc/php-fpm.conf at line 125. 
ERROR:. No pool defined at least one pool section must be specified in config file 
ERROR: failed to post process the configuration 
ERROR: FPM initialization failed

solution:

cp www.conf.default www.conf

4.ERROR: [pool www] can not get gid for group &apos;nobody&apos; 
ERROR: FPM initialization failed

solution:

Www.conf open files, user and group users into nginx default settings, usually the default is www-data.

  

#

[Official documentation page](https://www.php.net/manual/en/install.unix.nginx.php)

**[To root](/README.md)**