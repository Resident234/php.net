# Installation



Note, for Ubuntu, simply installing php5-mcrypt did not get mcrypt to work. You need to execute the following commands as root to enable it:<br><br>apt-get install php5-mcrypt<br>mv -i /etc/php5/conf.d/mcrypt.ini /etc/php5/mods-available/<br>php5enmod mcrypt<br>service apache2 restart  

---

If you don&apos;t have a /etc/php5/conf.d directory, you can simply only do: php5enmod mcrypt<br><br>Should be working fine.  

---

I needed to install mcrypt on Mac OS X Mavericks 10.9 for installing the Laravel 5 framework. I entered in the Terminal command line:<br><br>brew tap josegonzalez/homebrew-php<br>brew install php54 php54-mcrypt<br><br>This installed Mcrypt.  In Terminal type php -i to see a list of everything installed or for much better formatting and easier to read make a phpinfo.php page with this inside 

```
<?php phpinfo(); ?>
```
  <br><br>More help with installing on OS X:<br>http://stackoverflow.com/questions/14595841/installing-mcrypt-extension-for-php-on-osx-mountain-lion  

---

[Official documentation page](https://www.php.net/manual/en/mcrypt.installation.php)

**[To root](/README.md)**