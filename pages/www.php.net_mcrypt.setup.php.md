# Installing/Configuring




<div class="phpcode"><span class="html">
debian/ubuntu php5 (&gt;= 5.4.0~rc6-1) has introduced two new commands:<br>php5enmod and php5dismod<br><br># install the extension<br>sudo apt-get install php5-mcrypt<br># you can see that it&apos;s installed by the presence of the .ini file<br>cat /etc/php5/mods-available/mcrypt.ini<br># enable it<br>sudo php5enmod mcrypt<br># reload Apache to make use of the extension<br>sudo service apache2 reload</span>
</div>
  

#


<div class="phpcode"><span class="html">
Same Problem on Linux Mint - Call to undefined function mcrypt_create_iv...<br><br>Solved by adding the folowing line to the php.ini<br>extension=mcrypt.so<br><br>After that a <br>service apache2 restart<br>solved it...<br><br>Have fun with it...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mcrypt.setup.php)

**[To root](/README.md)**