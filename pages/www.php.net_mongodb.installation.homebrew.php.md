# Installing the MongoDB PHP Driver on macOS with Homebrew



How to install on macOS Mojave<br><br>Start with:<br><br>sudo pecl install mongodb<br><br>To check if mongodb package is installed, look for "mongodb" when you run:<br><br>pecl list<br><br>To get your installed mongodb.so path, run:<br><br>pecl list-files mongodb | grep mongodb.so<br><br>then remove (or comment out) on your php.ini file:<br><br>extension="mongodb.so" <br><br>(I could not found a line with extension="php_mongodb.so")<br><br>now insert a line with:<br><br>extension="{{the path to your installed mongodb.so}}"<br><br>Run this command to get your ext-*.ini directory path:<br><br>php -i | grep Scan

Create your ext-mongodb.ini with:

touch {{your conf.d path}}/ext-mongodb.ini

like: 
touch /usr/local/etc/php/7.1/conf.d/ext-mongodb.ini

Restart your apache to read the new configuration

Sanity check with:

php -i | grep mongodb 

Your&apos;re ready to go.?>
```
  

#

The mongodb install has been removed from Homebrew. To install the mongodb extension you need to use pecl.<br><br>sudo pecl install mongodb<br><br>You may need to follow some additional configuration. Follow the inline instructions as they appear. Once the install is finished it will add two lines at the bottom of your php.ini file. Instead, remove these lines and add the and add a separate file to your conf.d directory in the same directory as you php.ini file.<br><br>In php.ini remove<br><br>extension="mongodb.so" // remove<br>extension="php_mongodb.so // remove <br><br>Then run:<br><br>$ touch /usr/local/etc/php/5.6/conf.d/ext-mongodb.ini<br><br>Finally add this line to the new file:<br><br>// example<br>extension="/usr/local/Cellar/php@5.6/5.6.35/pecl/20131226/mongodb.so"  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.installation.homebrew.php)

**[To root](/README.md)**