# Installation





==Installation ibm_db2 in PHP5, using Data Server Driver Package and pecl on Debian / Ubuntu==

==Advantages==
#You do not need DB2 (database) installed
#The ibm_db2 driver is downloaded and automatically compiled by pecl

==Steps==
#1- Install packages
apt-get install php-pear ksh zip

#2- Make directory
mkdir /opt/ibm 

#3- Download Data Server Driver Package (dsdriver), as the architecture
(https://www-304.ibm.com/support/docview.wss?rs=4020&amp;uid=swg27016878&amp;wv=1)

#4- Decompress dsdriver at /opt/ibm/
tar -xvf v10.5fp1_linuxx64_dsdriver.tar.gz&#xA0; (linux64)
or
tar -xvf v10.5fp1_linuxia32_dsdriver.tar.gz (linux32)

#5- Change permission instalation script&#xA0; -&#xA0; /opt/ibm/dsddriver
chmod 755 installDSDriver

#6- Run the installation script 
ksh installDSDriver

#7- Download and install the driver using the pecl
pecl install ibm_db2

downloading ibm_db2-1.9.5.tgz ...
Starting to download ibm_db2-1.9.5.tgz (157,720 bytes)
................done: 157,720 bytes
5 source files, building
running: phpize
Configuring for:
PHP Api Version:&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 20090626
Zend Module Api No:&#xA0; &#xA0; &#xA0; 20090626
Zend Extension Api No:&#xA0;&#xA0; 220090626

#8- Configure the installation directory
DB2 Installation Directory? : /opt/ibm/dsdriver

Build process completed successfully
Installing &apos;/usr/lib/php5/20090626/ibm_db2.so&apos;
install ok: channel://pecl.php.net/ibm_db2-1.9.5
configuration option &quot;php_ini&quot; is not set to php.ini location
You should add &quot;extension=ibm_db2.so&quot; to php.ini

#9- Change php.ini
vim /etc/php5/apache2/php.ini
;;;;;;;;;;;;;;;;;;;;;;
; Dynamic Extensions ;
;;;;;;;;;;;;;;;;;;;;;;
extension = ibm_db2.so
extension = /usr/lib/php5/20090626/ibm_db2.so

#10- Reboot the Apache
service apache2 restart

  

#

[Official documentation page](https://www.php.net/manual/en/ibm-db2.installation.php)

**[To root](/README.md)**