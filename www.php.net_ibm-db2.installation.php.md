# Installation




<div class="phpcode"><span class="html">
==Installation ibm_db2 in PHP5, using Data Server Driver Package and pecl on Debian / Ubuntu==<br><br>==Advantages==<br>#You do not need DB2 (database) installed<br>#The ibm_db2 driver is downloaded and automatically compiled by pecl<br><br>==Steps==<br>#1- Install packages<br>apt-get install php-pear ksh zip<br><br>#2- Make directory<br>mkdir /opt/ibm <br><br>#3- Download Data Server Driver Package (dsdriver), as the architecture<br>(<a href="https://www-304.ibm.com/support/docview.wss?rs=4020&amp;uid=swg27016878&amp;wv=1" rel="nofollow" target="_blank">https://www-304.ibm.com/support/docview.wss?rs=4020&amp;uid=swg27016878&amp;wv=1</a>)<br><br>#4- Decompress dsdriver at /opt/ibm/<br>tar -xvf v10.5fp1_linuxx64_dsdriver.tar.gz&#xA0; (linux64)<br>or<br>tar -xvf v10.5fp1_linuxia32_dsdriver.tar.gz (linux32)<br><br>#5- Change permission instalation script&#xA0; -&#xA0; /opt/ibm/dsddriver<br>chmod 755 installDSDriver<br><br>#6- Run the installation script <br>ksh installDSDriver<br><br>#7- Download and install the driver using the pecl<br>pecl install ibm_db2<br><br>downloading ibm_db2-1.9.5.tgz ...<br>Starting to download ibm_db2-1.9.5.tgz (157,720 bytes)<br>................done: 157,720 bytes<br>5 source files, building<br>running: phpize<br>Configuring for:<br>PHP Api Version:&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 20090626<br>Zend Module Api No:&#xA0; &#xA0; &#xA0; 20090626<br>Zend Extension Api No:&#xA0;&#xA0; 220090626<br><br>#8- Configure the installation directory<br>DB2 Installation Directory? : /opt/ibm/dsdriver<br><br>Build process completed successfully<br>Installing &apos;/usr/lib/php5/20090626/ibm_db2.so&apos;<br>install ok: channel://pecl.php.net/ibm_db2-1.9.5<br>configuration option &quot;php_ini&quot; is not set to php.ini location<br>You should add &quot;extension=ibm_db2.so&quot; to php.ini<br><br>#9- Change php.ini<br>vim /etc/php5/apache2/php.ini<br>;;;;;;;;;;;;;;;;;;;;;;<br>; Dynamic Extensions ;<br>;;;;;;;;;;;;;;;;;;;;;;<br>extension = ibm_db2.so<br>extension = /usr/lib/php5/20090626/ibm_db2.so<br><br>#10- Reboot the Apache<br>service apache2 restart</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ibm-db2.installation.php)

**[To root](/README.md)**