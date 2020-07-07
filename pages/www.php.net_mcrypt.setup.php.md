# Installing/Configuring





debian/ubuntu php5 (&gt;= 5.4.0~rc6-1) has introduced two new commands:
php5enmod and php5dismod

# install the extension
sudo apt-get install php5-mcrypt
# you can see that it&apos;s installed by the presence of the .ini file
cat /etc/php5/mods-available/mcrypt.ini
# enable it
sudo php5enmod mcrypt
# reload Apache to make use of the extension
sudo service apache2 reload

  

#



Same Problem on Linux Mint - Call to undefined function mcrypt_create_iv...

Solved by adding the folowing line to the php.ini
extension=mcrypt.so

After that a 
service apache2 restart
solved it...

Have fun with it...

  

#

[Official documentation page](https://www.php.net/manual/en/mcrypt.setup.php)

**[To root](/README.md)**